## Redis Operations

### Fecha: 18/07/2024

<img src="images/redis.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - Mapping : Redis | Javascript
    - string | string 
    - list | string[]
    - set | string[]
    - hash | object {key: string}
    - float | string 
    - integer | number
  - Métodos para setear/obtener datos para cada tipo de dato. 
  - lrem, lrange, smembers son O(n), cuidado.
- **Código:**
  ```javascript
  const config = require('better-config');

  config.set('../config.json');

  const redis = require('../src/daos/impl/redis/redis_client');

  const client = redis.getClient();

  const testSuiteName = 'data_types';
  const testKeyName = `${testSuiteName}:test`;

  /* eslint-disable no-undef */

  beforeAll(() => {
    jest.setTimeout(60000);
  });

  beforeEach(async () => {
    await client.delAsync(testKeyName);
  });

  afterEach(async () => {
    await client.delAsync(testKeyName);
  });

  afterAll(async () => {
    // Release Redis connection.
    client.quit();
  });

  test(`${testSuiteName}: Test Redis string`, async () => {
    await client.setAsync(testKeyName, 'hello');

    const value = await client.getAsync(testKeyName);

    expect(typeof (value)).toBe('string');
    expect(value).toBe('hello');
  });

  test(`${testSuiteName}: Test Redis list`, async () => {
    await client.rpushAsync(testKeyName, 'one', 'two', 'three');

    const value = await client.lrangeAsync(testKeyName, 0, 3);

    expect(typeof (value)).toBe('object');
    expect(Array.isArray(value)).toBe(true);
    expect(value).toEqual(['one', 'two', 'three']);
  });

  test(`${testSuiteName}: Test Redis set`, async () => {
    await client.saddAsync(testKeyName, 'one', 'two', 'two', 'three');

    const value = await client.smembersAsync(testKeyName);

    expect(typeof (value)).toBe('object');
    expect(Array.isArray(value)).toBe(true);
    expect(value.length).toBe(3);
  });

  test(`${testSuiteName}: Test Redis hash`, async () => {
    await client.hmsetAsync(testKeyName, {
      name: 'John Doe',
      age: 42,
    });

    const value = await client.hgetallAsync(testKeyName);

    expect(typeof (value)).toBe('object');
    expect(Array.isArray(value)).toBe(false);
    expect(typeof (value.age)).toBe('string');
    expect(parseInt(value.age, 10)).toBe(42);
    expect(value).toEqual({
      name: 'John Doe',
      age: '42',
    });
  });

  test(`${testSuiteName}: Test Redis float`, async () => {
    await client.setAsync(testKeyName, 22.5);

    const value = await client.incrbyfloatAsync(testKeyName, 1);
    expect(typeof (value)).toBe('string');
    expect(value).toBe('23.5');
    expect(parseFloat(value)).toBeCloseTo(23.5);
  });

  test(`${testSuiteName}: Test Redis integer`, async () => {
    await client.setAsync(testKeyName, 22);

    let value = await client.getAsync(testKeyName);
    expect(typeof (value)).toBe('string');
    expect(value).toBe('22');

    value = await client.incrbyAsync(testKeyName, 1);
    expect(typeof (value)).toBe('number');
    expect(value).toBe(23);
    expect(parseInt(value, 10)).toBe(23);
  });

  /* eslint-enable */
  ```

## Recursos Adicionales
- [MongoDB Node Developer Path](https://learn.mongodb.com/learn/learning-path/mongodb-nodejs-developer-path)
