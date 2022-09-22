# roq-dec

A minimal crate for decoding ROQ video streams in Rust

## Usage

Add the following line to your Cargo.toml dependencies:

```
roq-dec = "0.1.0"
```

Create a ROQ decoder from a file:

```
let roqfile = File::open("path/to/video.roq").expect("Failed to open file");
let mut roqdec: RoqDecoder<File,u32,ColorspaceRgba8888> = RoqDecoder::new(roqfile).expect("Failed to initialize ROQ decoder");
```

Reading data from a ROQ decoder:

```
match roqdec.read_next()? {
    RoqEvent::InitVideo => {
        println!("Video dimensions: ({}, {}), Framerate: {}", roqdec.width, roqdec.height, roqdec.framerate);
    },
    RoqEvent::Video(framedata) => {
        // framedata is an array of width*height of pixel color type (using ColorspaceRgba8888, this will be &[u32])
    },
    RoqEvent::Audio(channels, samples) => {
        // channels will be either 1 or 2, samples is &[u16]
    },
    RoqEvent::Custom(chunk_id, chunk_arg, chunk_data) => {
        // handle custom chunk types here
        // chunk_data is Vec<u8> of chunk data
    },
    RoqEvent::EndOfFile => {
        // reached end of file
    }
};
```