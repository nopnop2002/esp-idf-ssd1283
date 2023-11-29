set(srcs "main.c"
	"ssd1283.c"
	"fontx.c"
	"decode_png.c"
    "decode_jpeg_v5.c"
	"pngle.c"
	)

idf_component_register(SRCS ${srcs} INCLUDE_DIRS ".")
