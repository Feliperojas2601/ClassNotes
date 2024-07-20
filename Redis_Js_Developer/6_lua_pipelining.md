## Redis Lua Pipelining Transactions

### Fecha: 19/07/2024

<img src="images/redis.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - Para evitar condiciones de carrera como una actualización desde dos clientes diferentes. Para esta atomicidad entre dos operaciones o más está Lua.
  - Para evitar múltiples round trips a Redis podemos usar pipelining o transactions.
  - Redis Lua scripts son como procedimientos almacenados. Ejecutar custom logic en el server Redis, de manera auto. En node vamos a manejar un módulo por script de Lua para organizar, hacer una función para cargar y cachear su hash para después ejecutarlo.
  - Pipelining es una feature del protocolo Redis que permite ejecutar múltiples comandos en un solo round trip al server. Se pueden combinar reads o write operations. Retorna todas las respuestas al mismo tiempo, para cada operación. No se ejecuta atomico.
  - Una transacción es un pipeline cuyos comandos están garantizados de ejecutarse como una operación átomica. Node redis ya lo maneja así usando client.multi() en lugar de client.batch(), bloqueando otros clientes y generando atomicidad.
- **Código:**
  ```lua
  -- Lua script to compare set if lower
  -- key: the name a Redis string storing a number
  -- new: a number which, if smaller, will replace the existing number
  local key = KEYS[1]
  local new = ARGV[1]
  local current = redis.call('GET', key)
  if (current == false) or
    (tonumber(new) < tonumber(current)) then
    redis.call('SET', key, new)
    return 1
  else
    return 0
  end
  ```
  ```javascript
  const redis = require('../redis_client');

  let sha;

  /**
  * Get the Lua source code for the script.
  * @returns {string} - Lua source code for the script.
  * @private
  */
  const getSource = () => `
    local key = KEYS[1]
    local new = ARGV[1]
    local current = redis.call('GET', key)

    if (current == false) or (tonumber(new) < tonumber(current)) then
      redis.call('SET', key, new)
      return 1
    else
      return 0
    end;
  `;

  const load = async () => {
    const client = redis.getClient();

    // Load script on first use...
    if (!sha) {
      sha = await client.scriptAsync('load', getSource());
    }

    return sha;
  };

  const updateIfLowest = (key, value) => [
    sha, // Script SHA
    1, // Number of Redis keys
    key,
    value,
  ];

  module.exports = {
    /**
    * Load the script into Redis and return its SHA.
    * @returns {string} - The SHA for this script.
    */
    load,

    /**
    * Build up an array of parameters that evalsha will use to run
    * an atomic compare and update if lower operation.
    *
    * @param {string} key - Redis key that the script will operate on.
    * @param {number} value - Value to set the key to if it passes the
    *   comparison test.
    * @returns {number} - 1 if the update was performed, 0 otherwise.
    */
    updateIfLowest,
  };

  const config = require('better-config');

  config.set('../config.json');

  const redis = require('../src/daos/impl/redis/redis_client');

  const client = redis.getClient();

  const testSuiteName = 'pipeline';
  const testKeyPrefix = `test:${testSuiteName}`;

  /* eslint-disable no-undef */

  beforeAll(() => {
    jest.setTimeout(60000);
  });

  afterEach(async () => {
    const testKeys = await client.keysAsync(`${testKeyPrefix}:*`);

    if (testKeys.length > 0) {
      await client.delAsync(testKeys);
    }
  });

  afterAll(() => {
    // Release Redis connection.
    client.quit();
  });

  test(`${testSuiteName}: example command pipeline`, async () => {
    const pipeline = client.batch();
    const testKey = `${testKeyPrefix}:example_pipeline`;
    const testKey2 = `${testKeyPrefix}:example_pipeline_2`;

    pipeline.hset(testKey, 'available', 'true');
    pipeline.expire(testKey, 1000);
    pipeline.sadd(testKey2, 1);

    const responses = await pipeline.execAsync();

    expect(responses).toHaveLength(3);
    expect(responses[0]).toBe(1);
    expect(responses[1]).toBe(1);
    expect(responses[2]).toBe(1);
  });

  test(`${testSuiteName}: example pipeline with bad command`, async () => {
    const pipeline = client.batch();
    const testKey = `${testKeyPrefix}:example_bad_pipeline`;

    pipeline.set(testKey, 1);
    pipeline.incr(testKey, 99); // Error, wrong number of arguments!
    pipeline.get(testKey);

    const responses = await pipeline.execAsync();

    expect(responses).toHaveLength(3);
    expect(responses[0]).toBe('OK');
    expect(responses[1]).toEqual({
      command: 'INCR',
      args: [
        testKey,
        99,
      ],
      code: 'ERR',
      position: 1,
    });
    expect(responses[2]).toBe('1');
  });

  /* eslint-enable */

  const config = require('better-config');

  config.set('../config.json');

  const redis = require('../src/daos/impl/redis/redis_client');

  const client = redis.getClient();

  const testSuiteName = 'transaction';
  const testKeyPrefix = `test:${testSuiteName}`;

  /* eslint-disable no-undef */

  beforeAll(() => {
    jest.setTimeout(60000);
  });

  afterEach(async () => {
    const testKeys = await client.keysAsync(`${testKeyPrefix}:*`);

    if (testKeys.length > 0) {
      await client.delAsync(testKeys);
    }
  });

  afterAll(() => {
    // Release Redis connection.
    client.quit();
  });

  test(`${testSuiteName}: example transaction`, async () => {
    const transaction = client.multi();
    const testKey = `${testKeyPrefix}:example_transaction`;
    const testKey2 = `${testKeyPrefix}:example_transaction_2`;

    transaction.hset(testKey, 'available', 'true');
    transaction.expire(testKey, 1000);
    transaction.sadd(testKey2, 1);

    const responses = await transaction.execAsync();

    expect(responses).toHaveLength(3);
    expect(responses[0]).toBe(1);
    expect(responses[1]).toBe(1);
    expect(responses[2]).toBe(1);
  });

  /* eslint-enable */
  ```

## Recursos Adicionales
- [Redis Js Developers course](https://university.redis.com/courses/ru102js/)
