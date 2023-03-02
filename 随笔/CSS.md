问题描述：样式设置图片宽高后，图片缩小变的模糊了。如何解决图片模糊问题？

解决办法：样式表中加入以下样式即可解决

img {
	image-rendering:-moz-crisp-edges;
	image-rendering:-o-crisp-edges;
	image-rendering:-webkit-optimize-contrast;
	image-rendering: crisp-edges;
	-ms-interpolation-mode:nearest-neighbor;
}