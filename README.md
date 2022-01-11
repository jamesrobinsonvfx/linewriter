# Linewriter

Linewriter is a simple SOP that adds some functionality around the built-in Font
SOP. It is most useful for creating text labels in the viewport for recording
parameter values when flipbooking wedges etc.

> Compatible with Python 3 versions of Houdini 19.0. If there is enough interest
> for older versions, I'll post one for H18.5.

> On macOS with UI scaling on, some parameters may not line up in a pretty way in
> Houdni 19.0. This has been reported to SideFX.


## Problems with built-in Font SOP

There are a few slightly annoying gotchas when making HUDs/Overlays out of the
Font SOP for recoding parameter values etc.

### 1. Font SOP doesn't play well with channel references

When you're building a HUD/Overlay with the default Font SOP, it can be tedious
adding channel references, since when you RMB > Paste Relative References, it
tends to blast away everything you had in there already. So instead, you need to
copy the path to the parm someplace else, and paste it in.

Linewriter uses a single-line string parameter, where this doesn't seem to be an
issue.

### 2. Float values are over-precise

[This blog post](https://www.jamesrobinsonvfx.com/tips/2021/08/19/ftrim-function/)
goes over this more, but in short - a lot of times you see what should be a simple
parm value like `0.025` being represented in a string parm as
`0.025000000000000001`. This is distracting, and wastes a lot of on-screen space.

Each line of line-writer has a button [button pic] that tries to "ftrimify" your
line. It looks for each `ch()` or `chs()`, and wraps them up in an `ftrim()`.
This helps cut down the numbers to max of six floating point digits. There is
also a button under the **Utilities** folder that will run it over all the lines
at once.


### 3. Font artifacts

Sometimes you'll make a nice looking overlay, only to have a few B's or D's get
their holes filled in. They look fine at the origin, but when parented to the
camera, they look all wrong.

I'm not sure what the exact cause is (some sort of precision thing I imagine),
but the solution is simple: Pack up your Font SOP at the end. All the
characters with holes will appear as they did before transforming (Linewriter
does this per-line, as well as one final pack at the end).

Other times, the artifacts are a direct result of the Font SOP itself. In this
case, there is also an option to fix these holes, though it does decrease performance.

### 4. Limited coloring options

If you have a few key lines you want to stand out, you have to either make a few
extra Font SOPs, or group the characters manually and color them later.
Linewriter lets you set a base color for the whole block, and override per-line as needed.

### 5. Performance

In a single Font SOP, if even one of your referenced parameters is animated
(time-dependent), it will cause the whole thing to recook each frame. For single
lines, this isn't a big deal. But with bigger, monolithic Font SOPs holding a
whole slew of data for your overlay, this can be a little bit of a bottleneck.

Linewriter optimizes this by splitting out lines that are time-dependent from
the ones that aren't, so only lines with animated data will recook.

### 6. No background color options

Look at Nuke's Text node options and you'll see a tab called Background. This is
very useful for when your text is occluding some of your scene, and becomes
difficult to read. Linewriter adds an optional background behind each line to
make sure the text stands out.

## Other Features

* Help Card
* Embedded example setup
  > Extra > Load Example Setup