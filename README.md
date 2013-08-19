# ellipso
## Introduction
Last christmas my girlfriend bought me a [Sphero](http://gosphero.com/) which I spent a few weeks
messing about with, and then put it in a drawer.  I've pulled it back out and built this library to
play with some Clojure code.  This library provides basic integration with the Sphero device,
implementing the protocol documented as [Orbotix Communication API](https://github.com/orbotix/DeveloperResources/blob/master/docs/Sphero_API_1.46.pdf?raw=true).

## Usage
All of this is subject to change but:

    (require '[ellipso.core :as core])
    (def sphero (core/connect "/dev/tty.Sphero-RBR-RN-SPP"))

You'll need to change the connect string to the one for your Sphero.

    (require '[ellipso.commands :as commands])
    (def rainbow
      (map commands/colour [0xFF0000 0xFF8000 0xFFFF00 0x00FF00 0x0000FF 0x8000FF 0xFF00FF]))
    (def spin
      (let [speeds (range 0x30 0xFF 0x30)
            cycle  (concat speeds (reverse speeds))]
        (map (partial commands/spin commands/CLOCKWISE) cycle))))

    (reduce commands/execute sphero (interpose (commands/pause 1000) rainbow spin))

Best to review the code in the `ellipso.commands` namespace.  Basically commands
are functions that are applied to a `ellipso.core/Sphero` instance.

## TODO
* look more into the state monad because that's what is needed;
* work out how to do [The Batman Curve](http://mathworld.wolfram.com/BatmanCurve.html)!

## License

Copyright © 2013 Matthew Denner

Distributed under the Eclipse Public License either version 1.0 or (at your option) any later version.
