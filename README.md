# Path Flattening Example
This shows how to use the rive.tools.pure.js file built by https://github.com/rive-app/rive-wasm. To build a new version, clone https://github.com/rive-app/rive-wasm and do:

```
cd wasm
./build-js.sh tools
```

The file will show up in wasm/publish.

## Path Flattening
Flattening the path is sort of a misnomer, but I couldn't think of something better. It basically takes a Rive path which can be made up of different kinds of cubic vertices, corner vertices, vertices with radius, bound to bones, etc and converts them to a simple world/artboard space (no transforms) path made up of simple points (x, y) and cubic points (x, y, inX, inY, outX, outY).

You can flatten a path in an artboard by providing its index int the artboard objects list (effectively its id in the artboard). For example, for the provided flatten.riv, there's a path at index 2:
```
const flatPath = artboard.flattenPath(2);
```

You can now use flatPath to iterate vertices, query their type, and get their coordinates. Note the API is hideous, but this is meant to be a tool, we didn't want to need to expose a bunch of different objects for something rarely used (although if someone finds values in it, we of course can...).

```
for (let i = 0; i < flatPath.length(); i++)
{
    if (flatPath.isCubic(i))
    {
        // You can get the in/out positions:
        const in = [flatPath.inX(i), flatPath.inY(i)];
        const out = [flatPath.outX(i), flatPath.outY(i)];
    }
    // All points have x/y translation
    const translation = [flatPath.x(i), flatPath.y(i)];
}
```

## Example
There's an example in index.html that shows how to query and draw the points. It draws corners in white, cubics in yellow, and cubic control (in/out) in red.