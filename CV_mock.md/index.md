# Improved API latency by 40% and reduced database load by 69% through Redis caching.

At RNGplay, I worked with my team on persistent multiplier functionality for the Base Game of a slot game called Shogun.

The multiplier accumulated across spins, so each new spin needed access to the previously accumulated value. We used Redis to retrieve the previous multiplier quickly and stored the updated multiplier in Redis so it could be retrieved during the next spin. This helped avoid repeatedly querying MySQL for the same multiplier data, reducing repeated database reads and helping improve API latency.

We also had a reset rule. When a spin's winning amount exceeded 1,000, the multiplier needed to become zero and disappear from the symbols. In that case, I cleared the Redis cache associated with the game multiplier, rather than clearing the entire Redis cache. The corresponding data in MySQL was also reset.

According to my CV, API latency improved by approximately 40%, and database load decreased by approximately 69%. These figures are approximate, and I don't recall the exact measurement methodology.