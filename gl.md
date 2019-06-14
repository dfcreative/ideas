* Use uniforms for per-frame changes, constant attributes - for more often changes.
* Each fragment thread is (expectedly) 3-5 times slower than CPU, so sometimes, especially when it comes to conditioning, it is better to do CPU processing and leave really hugely parallel but simple math.
	* The logic is simple: if we can do something with CPU once - (same as we would do it with GPU) - better leave it to CPU. GPU will take tripled time + awaiting for slowest fragment.
* bufferSubData and bufferData are times faster thant texImage2D
* antialias flag drops FPS twice
* Use highp for iphone (digits may error with lowp)
