# Splitting Mackie ProFXv3 channels on Manjaro

Mackie does not have drivers for linux, and its main output is on channels 3 & 4. Manjaro sees the mixer as a 4.0 surround device instead so we need to split channels 1-2 and 3-4.\
\
Assumptions: \
\- You're using [Pipewire](https://pipewire.org/).

## Splitting the channels:

1. Find the node name of your card. Just look through what `pw-cli list-objects | grep alsa_output` returns. One of them is your device's `node.name` .
2. Copy the pipewire config file from `/usr/share/pipewire.conf` to `~/.config/pipewire/pipewire.conf`.
3. Install helvum and check the names of the playback channels of the device. it will be a bunch of `playback_<x>`. The x is important later. Figure out which x is which physical output.
4. Inside the newly created config file we're gonna create two loopback devices inside the `context.modules` section.

<pre class="language-systemd" data-title="~/.config/pipewire/pipewire.conf"><code class="lang-systemd"><strong>
</strong><strong>...
</strong><strong>
</strong><strong>{   
</strong>    name = libpipewire-module-loopback
    args = {        
        node.description = "&#x3C;Pick a description for your stereo output>"
        capture.props = {
            node.name = "&#x3C;Pick a name>"
            media.class = "Audio/Sink"
            audio.position = [ FL FR ]
        }
        playback.props = {
            node.name = "playback.&#x3C;Name you picked>"
            audio.position = [ &#x3C;the x's from step 3 go here> ]
            node.target = "&#x3C;the node name from step 1 goes here>"
            stream.dont-remix = true
            node.passive = true
        }
    }
}

...

</code></pre>

<div align="left">

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p>You might need to play around with the choice of the output channels (<code>playback.props.audio.position</code>) to match your physical outputs in a way that makes sense. For me I mapped the output from <code>[ FL FR ]</code> (channel 1-2)  to <code>[ RL RR ]</code> (channel 3-4).  <br>I discarded output channels 1-2 because I don't use them, so I only created one loopback device.</p></figcaption></figure>

</div>

5. When you have added both loopback devices to the config, reload pipewire with `systemctl --user restart pipewire.service`. Check in helvum (restart the app after restarting pipewire) if the loopback devices map to the correct outputs and test it with a test tone or some other audio to make sure.
6. Once you have figured out the right mapping you're done. Don't forget to set your system audio to the loopback device instead of the raw device. This config is loaded every time pipewire starts.







Original source: [https://www.reddit.com/r/linuxquestions/comments/vrimtn/splitting\_a\_pipewire\_alsa\_nodes\_channels/ievs15y/](https://www.reddit.com/r/linuxquestions/comments/vrimtn/splitting\_a\_pipewire\_alsa\_nodes\_channels/ievs15y/)
