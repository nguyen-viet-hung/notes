# ffmpeg

Using setting as below for generating constant bit rate content

```c
AVCodecContext* ctx;
....
ctx->rc_max_rate = ctx->rc_buffer_size = ctx->bit_rate = bit_rate;
ctx->rc_initial_buffer_occupancy = -1;
ctx->qcompress = 0.0
ctx->qmin = 27;
ctx->qmax = 35;
ctx->max_qdiff = 3;
```