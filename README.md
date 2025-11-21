# SpritesetAnimationTexture

A Godot addon that converts `SpriteFrames` animations into usable `Texture2D` resources. Use animated sprites anywhere textures are accepted—UI elements, materials, shaders, draw calls, and nested within other animated textures. **Plug-and-play**: simply drop it into your project and it works immediately.

[![View video](https://img.youtube.com/vi/jq-kXXF9kVs/0.jpg)](https://www.youtube.com/watch?v=jq-kXXF9kVs)

## Features

🟢 **Universal Texture Support** - Use animated sprites anywhere `Texture2D` is accepted (Sprite2D, TextureRect, Button icons, materials, shaders, etc.)

🟢 **Nested Animations** - Stack animated textures inside other animated textures up to 7 levels deep for complex visual effects

🟢 **Editor & Runtime** - Seamless functionality in both the Godot editor and at runtime

🟢 **Automatic Synchronization** - Handles frame updates, caching, and rendering across all instances automatically

🟢 **Zero Configuration** - No plugin activation needed—just add to your project

🟢 **Memory Efficient** - Automatic cleanup of orphaned references and intelligent caching

🟢 **Performance Optimized** - Hibernation mode for off-screen animations, stack depth protection against infinite loops

## Installation

1. **Download or clone** this addon into your project's `addons/` folder:

```
your_project/
└── addons/
    └── AnimateTexture/
        ├── plugin.cfg
        └── spriteset_animation_texture.gd
```

2. **That's it.** No plugin activation needed—`SpritesetAnimationTexture` is immediately available as an export option in any texture property across your entire project.

## Quick Start

### Basic Usage (Code)

```gdscript
# Create and configure
var animated_texture = SpritesetAnimationTexture.new()
animated_texture.sprite_frames = preload("res://animations/player.tres")
animated_texture.animation = "idle"
animated_texture.playing = true

# Use anywhere a texture is needed
$Sprite2D.texture = animated_texture
$TextureRect.texture = animated_texture
$Button.icon = animated_texture
```

### Using the Inspector

1. Select any node with a texture export (Sprite2D, TextureRect, Panel, etc.)
2. In the Inspector, find the texture property
3. Create a new resource → choose `SpritesetAnimationTexture`
4. Assign your `SpriteFrames` resource
5. Select the animation name to play
6. Adjust `speed_scale` if needed (default: 1.0)
7. Check `autoplay` to start automatically

### Nested Animations

```gdscript
# Inner animation layer
var sparkle = SpritesetAnimationTexture.new()
sparkle.sprite_frames = preload("res://animations/sparkle.tres")
sparkle.animation = "twinkle"

# Outer animation that uses inner animation as frames
var magic_effect = SpritesetAnimationTexture.new()
magic_effect.sprite_frames = preload("res://animations/magic.tres")
magic_effect.animation = "cast"

# The magic effect animation will now display the sparkle animation
# as its frame sequence, creating a composite animation effect
$Effect.texture = magic_effect
```

**Supports up to 7 levels of nesting** for complex visual compositions.

## Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `sprite_frames` | SpriteFrames | null | The SpriteFrames resource containing animation data |
| `animation` | String | "" | Current animation name to play |
| `speed_scale` | float | 1.0 | Playback speed multiplier (min: 0.1, max: unlimited) |
| `autoplay` | bool | true | Automatically start playing when assigned to a node |
| `playing` | bool | true | Current playback state (read/write) |
| `current_frame` | int | 0 | Current frame index (read-only during playback) |

## Signals

```gdscript
# Emitted when a non-looping animation reaches its end
signal animation_finished(animation_name: String)

# Emitted whenever current_frame changes
signal frame_changed()
```

## Methods

```gdscript
# Play an animation (optionally starting from a specific frame)
play(p_animation: String = "", p_from_frame: int = 0) -> void

# Stop playback
stop() -> void

# Manually set the current frame
set_frame(frame: int) -> void

# Get the current frame index
get_frame() -> int

# Get total frame count for the current animation
get_frame_count() -> int

# Get the current animation name
get_animation() -> String

# Check if animation is currently playing
is_playing() -> bool

# Reset animation to first frame and current state
reset() -> void
```

## Use Cases

### Game Development

🟢 **Character animations** - Walk, run, idle, attack, death animations on sprites

🟢 **Visual effects** - Explosions, magic spells, particle effects

🟢 **UI animations** - Pulsing buttons, loading spinners, status indicators

🟢 **Game elements** - Collectibles, environmental effects, enemy animations

### UI/UX

🟢 **Animated buttons** - Glowing effects, hover animations

🟢 **Loading states** - Spinners, progress indicators

🟢 **Status displays** - Blinking alerts, pulsing status icons

🟢 **Transitions** - Animated screen overlays, visual feedback

### Advanced

🟢 **Layered effects** - Combine multiple animated textures for complex visuals

🟢 **Shader support** - Use animated textures in material shaders

🟢 **Dynamic scenes** - Change animations based on game state

🟢 **Performance optimization** - Control animation playback based on visibility

## Performance Considerations

🟢 **Caching System** - Textures are cached to minimize lookups and improve frame rate

🟢 **Hibernation Mode** - Animations automatically pause when nodes are deleted or the texture is unused, reducing memory and CPU overhead

🟢 **Stack Depth Protection** - Recursive nesting is limited to 7 levels to prevent infinite loops (max stack depth: 8)

🟢 **Automatic Cleanup** - Dead references are cleaned up periodically to prevent memory leaks

🟢 **Efficient Synchronization** - Updates are batched and synchronized with the rendering pipeline

### Tips for Best Performance

✅ Stop unused animations: Set `playing = false` for off-screen textures

✅ Preload SpriteFrames: Use `preload()` instead of `load()` for zero-delay initialization

✅ Cache instances: Reuse animated texture instances instead of creating new ones repeatedly

✅ Monitor frame rate: Very complex nested animations on many sprites may impact performance—profile your specific use case


## Editor Support

🟢 **Live Preview** - Changes to properties update instantly in the editor viewport

🟢 **Animation Selection** - Dropdown menu automatically populated from SpriteFrames animations

🟢 **Real-time Updates** - Modify animations while the scene is running and see changes instantly

🟢 **Property Validation** - Invalid animation names are automatically corrected

🟢 **Nested Texture Support** - The editor properly handles and displays nested animated textures

## Limitations & Notes

🟢 **Requires SpriteFrames** - An animated texture must have a valid `SpriteFrames` resource assigned

🟢 **Single Animation** - Only one animation can play at a time (assign different textures to different nodes for parallel animations)

🟢 **Nesting Depth** - Maximum 7 levels of nested animations to prevent infinite loops

🟢 **Tested on Latest Godot** - This addon is tested on the latest stable version of Godot

## Troubleshooting

### 🔴 Texture doesn't appear?

- Verify `sprite_frames` is assigned

- Check that `animation` name exists in the SpriteFrames resource

- Confirm `playing` is set to `true`

- Check the node is visible in the scene tree

### 🔴 Animation not updating?

- Ensure `sprite_frames` is a valid, loaded resource

- Verify the animation loop/duration settings in SpriteFrames

- Try calling `reset()` to reinitialize the animation state

### 🔴 Performance issues?

- Reduce the number of simultaneous animated textures

- Lower animation speed with `speed_scale`

- Stop animations for off-screen elements

- Use simpler sprite atlases with fewer frames

### 🔴 Nested textures not working?

- Ensure inner texture is a valid `SpritesetAnimationTexture`

- Check that inner texture has `sprite_frames` assigned

- Verify nesting depth is 7 levels or less

- Confirm inner animation names are valid

## Architecture Notes

This addon is highly optimized with strict internal ordering requirements:

- Variable positions and function call sequences are critical
- State synchronization between editor and runtime is precise
- The stack depth protection prevents infinite recursion
- Automatic cleanup prevents memory leaks

**Do not modify internal implementation details** without thorough testing. Use the public API and properties for customization.

## License

MIT License - Feel free to use in commercial and personal projects. See LICENSE file for details.

## Contributing

Found a bug? Have feature suggestions? Issues and pull requests are welcome!

**Before reporting an issue:**

✅ Test on the latest Godot version

✅ Provide a minimal reproducible example

✅ Include your Godot version and addon version

✅ Check if the behavior is documented in this README

## Changelog

### Version 1.0.0 (Initial Release)

✅ Universal Texture2D support across all Godot nodes

✅ Up to 7 levels of nested animations

✅ Editor and runtime functionality

✅ Automatic resource caching and cleanup

✅ Performance optimization with hibernation mode

✅ Full GDScript documentation

---

**Created for the Godot community.** If this addon saves you time, consider starring the repository!

**Requirements:**

- Godot (latest stable version)
- No external dependencies

**Status:** Stable and production-ready ✅
