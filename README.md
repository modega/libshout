# libshout-node

### Description
A fork of [libshout](https://github.com/native-bindings/libshout). Fixed errors and security vulnerabilities. Updated packages to the latest versions. Added better documentation and examples.

### Installation

```
npm i libshout-node
```

```
Tested on POP!_OS 22.04
```

```
NODE_INSTALL_DIR enviroment must be set
Requires pkconfig on the system to be installed
```

## Usage

### Options
- `setHost` - Icecast server host
- `setProtocol` - Icecast server protocol (0 - HTTP)
- `setPort` - Icecast server port
- `setPassword` - Icecast server strean password
- `setMount` - Icecast server stream mount
- `setUser` - Icecast server user (Default: source)
- `setFormat` - Audio format (0 - OGG, 1 - MP3)

Refer to detailed information in the [libshout documentation](https://github.com/xiph/Icecast-libshout/blob/master/doc/libshout.xml).

### Example 1 - OGG
```ts
import {Shout} from 'libshout-node';

async function run() {
    const shout = new Shout();
    
    shout.setHost('127.0.0.1');
    shout.setProtocol(0);
    shout.setPort(8000);
    shout.setPassword('icecast-stream-pass');
    shout.setMount('/icecast-stream-mount');
    shout.setUser('source');
    shout.setFormat(0);
    shout.open();
    
    const fd = await fs.promises.open(path.resolve(__dirname,'audio.ogg'), 'r');
    const tmp = Buffer.allocUnsafe(1024 * 4);
    
    while(1) {
        // Read audio data from file
        const {bytesRead} = await fd.read(tmp, 0, tmp.byteLength, null);
        if(bytesRead) {
            //Send audio data to icecast server
            shout.send(tmp.buffer.slice(0, bytesRead));
        } else {
            break;
        }
        shout.sync();
    }
    shout.close();
}
```

### Example 2 - MP3
```ts
import {Shout} from 'libshout-node';

async function run() {
    const shout = new Shout();
    
    shout.setHost('127.0.0.1');
    shout.setProtocol(0);
    shout.setPort(8000);
    shout.setPassword('icecast-stream-pass');
    shout.setMount('/icecast-stream-mount');
    shout.setUser('source');
    shout.setFormat(1);
    shout.open();
    
    const fd = await fs.promises.open(path.resolve(__dirname,'audio.mp3'), 'r');
    const tmp = Buffer.allocUnsafe(1024 * 4);
    
    while(1) {
        // Read audio data from file
        const {bytesRead} = await fd.read(tmp, 0, tmp.byteLength, null);
        if(bytesRead) {
            //Send audio data to icecast server
            shout.send(tmp.buffer.slice(0, bytesRead));
        } else {
            break;
        }
        shout.sync();
    }
    shout.close();
}
```

### Example 3 - Endless Stream
```ts
import {Shout} from 'libshout-node';

async function run() {
    const shout = new Shout();
    
    shout.setHost('127.0.0.1');
    shout.setProtocol(0);
    shout.setPort(8000);
    shout.setPassword('icecast-stream-pass');
    shout.setMount('/icecast-stream-mount');
    shout.setUser('source');
    shout.setFormat(0);
    shout.open();
    
    const fd = await fs.promises.open(path.resolve(__dirname,'audio.mp3'), 'r');
    const tmp = Buffer.allocUnsafe(1024 * 4);
    
    while(1) {
        // Read audio data from file
        const {bytesRead} = await fd.read(tmp, 0, tmp.byteLength, null);
        if(bytesRead) {
            //Send audio data to icecast server
            shout.send(tmp.buffer.slice(0, bytesRead));
        } else {
            //Send empty buffer to icecast server
            shout.send(tmp.buffer.slice(0, 1024*4));
        }
        shout.sync();
    }
    shout.close();
}
```