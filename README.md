Binary I/O library for Ruby that has an interface similar the C# Binary Reader and Writer.

# Reader

## Loading a file

```rb
require 'bin_tools'

f = BinTools::Reader.from_file 'test.bin'
```

## Setting a base position

Sets the base (origin) position, which the read pointer considers as 0.

```rb
f.set_base 0x8000
```

## Navigating to an offset

Seek to an offset (sets the read pointer).

If a base position was set, the offset in the file will be _base + offset_.

```rb
f.seek 0x4000
```

## Reading data

Reading values will auto-advance the read pointer by the data size of the value.

Read a byte (advances +1)

```
x = f.read_byte # unsigned
x = f.read_s8   # signed
```

Read a 16-bit integer (advances +2)

```
x = f.read_u16_le # unsigned, little endian
x = f.read_s16_le # signed, little endian
x = f.read_u16_be # unsigned, big endian
```

Read a 32-bit integer (advances +4)

```
x = f.read_u32_le # unsigned, little endian
x = f.read_s32_le # signed, little endian
x = f.read_u32_be # unsigned, big endian
```

Read a 32-bit float (advances +4)

```
x = f.read_f32_le # little endian
```

Read byte string of arbitrary length (advances +length)

```
str = f.read_bin 0x1000
str = f.read_binswap 0x1000 # reads and does a 32-bit byteswap
```

Read variable length quantity commonly [used in MIDI](https://en.wikipedia.org/wiki/Variable-length_quantity) (advances +length)

```
x = f.read_varlen_le # little endian
x = f.read_varlen_be # big endian (used in MIDI)
```

Debug print with file offset, read pointer offset

```
f.msg "GFX section start"
# "C000(4000): GFX section start"
```

Get current offset

```
f.seek 0x4000
f.read_byte
f.tell # => 0x4001
```

# Writer


