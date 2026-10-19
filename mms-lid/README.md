# mms-lid

Lana downloads `Xenova/mms-lid-1024` (int8, CC BY-NC 4.0) as published.

Dead end, don't retry: pooling encoder frames from one long pass per window
fails, because the encoder attends across its whole input. The app runs 3 s
windows separately.
