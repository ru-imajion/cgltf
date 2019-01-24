# cgltf
**Single-file/stb-style C glTF loader**

[![Build Status](https://travis-ci.org/jkuhlmann/cgltf.svg?branch=master)](https://travis-ci.org/jkuhlmann/cgltf)

## Usage

Loading from file:
```c
#include "cgltf.h"

cgltf_options options = {0};
cgltf_data* data = NULL;
cgltf_result result = cgltf_parse_file(&options, "scene.gltf", &data);
if (result == cgltf_result_success)
{
	/* TODO make awesome stuff */
	cgltf_free(data);
}
```

Loading from memory:
```c
#include "cgltf.h"

void* buf; /* Pointer to glb or gltf file data */
size_t size; /* Size of the file data */

cgltf_options options = {0};
cgltf_data* data = NULL;
cgltf_result result = cgltf_parse(&options, buf, size, &data);
if (result == cgltf_result_success)
{
	/* TODO make awesome stuff */
	cgltf_free(data);
}
```

Note that cgltf does not load the contents of extra files such as buffers or images into memory; you'll need to read these files yourself using URIs from `data.buffers[]` or `data.images[]` respectively.

## Support
cgltf supports this:
- glTF 2.0
- glb (binary files) and gltf (JSON files)
- meshes (including accessors, buffer views, buffers)
- materials (including textures, samplers, images)
- scenes and nodes
- skins
- animations
- cameras

cgltf does **not** yet support this:
- morph targets
- any extensions (like Draco, for example)

## Building
The easiest approach is to integrate the `cgltf.h` header file into your project. If you are unfamiliar with single-file C libraries (also known as stb-style libraries), this is how it goes:

1. Include `cgltf.h` where you need the functionality.
1. Have exactly one source file that defines `CGLTF_IMPLEMENTATION` before including `cgltf.h`.
1. Use the cgltf functions as described above.

For testing, there is a CMake file in test `test/` folder.

## Contributing
Everyone is welcome to contribute to the library. If you find any problems, you can submit them using [GitHub's issue system](https://github.com/jkuhlmann/cgltf/issues). If you want to contribute code, you should fork the project and then send a pull request.

## Dependencies
Basically none.

C headers being used:
```
#include <stdint.h>
#include <string.h>
#include <stdlib.h>
#include <stdio.h>
```

Note, this library has a copy of the [JSMN JSON parser](https://github.com/zserge/jsmn) embedded in its source.

## Testing
There is a Python script in the `test/` folder that retrieves the glTF sample files from the glTF-Sample-Models repository and runs the library against all gltf and glb files in there.

