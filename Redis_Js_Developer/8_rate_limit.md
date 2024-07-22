## Redis Rate limit 

### Fecha: 21/07/2024

<img src="images/redis.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - Limitar número de llamados a un API por usuario. Dos métodos, Fixed o Sliding window.
  - Fixed cuenta el número de requests en un intervalo de tiempo, en sligin cuenta el número de requests en un intervalo de 60 segs desde el inicio de la primera. Fixed es más fácil y ligera de implementar pero menos precisa.
  - Hacemos un schema en redis limiter:name:minute-block:max-hits.
- **Código:**
  ```javascript
  const redis = require('./redis_client');
  const keyGenerator = require('./redis_key_generator');

  /**
    * Record a hit against a unique resource that is being
    * rate limited.  Will return -1 when the resource has hit
    * the rate limit.
    * @param {string} name - the unique name of the resource.
    * @param {Object} opts - object containing interval and maxHits details:
    *   {
    *     interval: 1,
    *     maxHits: 5
    *   }
    * @returns {Promise} - Promise that resolves to number of hits remaining,
    *   or 0 if the rate limit has been exceeded..
    *
    * @private
    */
  const hitFixedWindow = async (name, opts) => {
    const client = redis.getClient();
    const key = keyGenerator.getRateLimiterKey(name, opts.interval, opts.maxHits);

    const pipeline = client.batch();

    pipeline.incr(key);
    pipeline.expire(key, opts.interval * 60);

    const response = await pipeline.execAsync();
    const hits = parseInt(response[0], 10);

    let hitsRemaining;

    if (hits > opts.maxHits) {
      // Too many hits.
      hitsRemaining = -1;
    } else {
      // Return number of hits remaining.
      hitsRemaining = opts.maxHits - hits;
    }

    return hitsRemaining;
  };

  module.exports = {
    /**
    * Record a hit against a unique resource that is being
    * rate limited.  Will return 0 when the resource has hit
    * the rate limit.
    * @param {string} name - the unique name of the resource.
    * @param {Object} opts - object containing interval and maxHits details:
    *   {
    *     interval: 1,
    *     maxHits: 5
    *   }
    * @returns {Promise} - Promise that resolves to number of hits remaining,
    *   or 0 if the rate limit has been exceeded..
    */
    hit: hitFixedWindow,
  };

  const hitSlidingWindow = async (name, opts) => {
    const client = redis.getClient();

    // START Challenge #7
    const key = keyGenerator.getKey(`limiter:${opts.interval}:${name}:${opts.maxHits}`);
    const now = timeUtils.getCurrentTimestampMillis();

    const transaction = client.multi();

    const member = `${now}-${Math.random()}`;

    transaction.zadd(key, now, member);
    transaction.zremrangebyscore(key, 0, now - opts.interval);
    transaction.zcard(key);

    const response = await transaction.execAsync();

    const hits = parseInt(response[2], 10);

    let hitsRemaining;

    if (hits > opts.maxHits) {
      // Too many hits.
      hitsRemaining = -1;
    } else {
      // Return number of hits remaining.
      hitsRemaining = opts.maxHits - hits;
    }

    return hitsRemaining;

    // END Challenge #7
  };
  ```

## Recursos Adicionales
- [Redis Js Developers course](https://university.redis.com/courses/ru102js/)
