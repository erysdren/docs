
# BSP Hulls

Levels in the Quake engine are built out of **brushes**, a somewhat nonsenical
term referring to a set of geometric plane equations that form a convex solid.
In lamens terms, any three dimensional shape that does not "cut into" itself
at any point.

In the following image, the red line indicates a **convex** shape while the
black line indicates a **concave** shape.

![](https://upload.wikimedia.org/wikipedia/commons/7/71/Simple_concave_polygon_Convex_Hull.svg)

Jespa, CC BY-SA 4.0 <https://creativecommons.org/licenses/by-sa/4.0>, via Wikimedia Commons

---

As levels are compiled from the `.map` format into the `.bsp` format, the
**QBSP** tool breaks down the brushes into their component faces, and removes
any faces that will not be visible from "inside" the level. It also cuts down
the other faces to only their visible area. During this process, the original
brush shapes are discarded.

For the Quake engine to handle physics and collision, some "hull" shapes must
be kept in the final BSP file. Instead of keeping the entire convex brush
shape, the QBSP tool "expands" the brushes to simulate collision for 3 distinct
"hull sizes". These hull sizes are:

| Name | Min | Max | Absolute | Notes |
|---|---|---|---|---|
| VEC_ORIGIN | `0 0 0` | `0 0 0` | `0 0 0` | This is an infinitesimally small point, used for small objects like grenades. |
| VEC_HULL1 | `-16 -16 -24` | `16 16 32` | `32 32 56` | This is the "small" hull, used for most entities including the player. |
| VEC_HULL2 |`-32 -32 -24` | `32 32 64` | `64 64 88` | This is the "large" hull, primarily used for the Shambler. |
