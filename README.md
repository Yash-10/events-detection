# Expanding Events Detection

**Project repository**: https://github.com/poliastro/poliastro/

This page describes the work done during Google Summer of Code 2021 with [poliastro](https://github.com/poliastro/poliastro). I worked on adding some space event detectors under the `twobody` problem by implementing raw orbital mechanics algorithms accelerated by the `numba.jit()` decorator and using the `events` parameter from `scipy.integrate.solve_ivp`.

The validation cases for some events were developed against the [`orekit`](https://www.orekit.org/) software, using the [Orekit Python Wrapper](https://gitlab.orekit.org/orekit-labs/python-wrapper) while others were tested using test examples from literature. The user guide for the added events along with some plotting results, using `poliastro`’s plotting capabilities, can be found [here](https://docs.poliastro.space/en/latest/examples/Detecting%20Events.html).

## Added events (so far…)

- Altitude Crossing (https://github.com/poliastro/poliastro/pull/1254).
- Latitude Crossing (https://github.com/poliastro/poliastro/pull/1268).
- Eclipse (penumbra and umbra) Event (https://github.com/poliastro/poliastro/pull/1246).
- Nodal Crossing (https://github.com/poliastro/poliastro/pull/1293).

These events could be used for orbits around any solar system body, and are not restricted to the Earth, since they do not leverage any earth-specific properties.

The template structure to use the events is given below:

```py
event_1 = <Event>(args, direction=0, terminal=True)
.
.
.
events = [<event_1>, <event_2>, …, <event_n>]
r, v = cowell(
	attractor.k,
	orbit.r,
	orbit.v,
	tofs,
	events=events,
	f=func_twobody
)
```
where the argument `f`, which defaults to keplerian-only forces, can be modified to include, for example, atmospheric perturbations for more rigorous orbit analyses.

## What is left to do?

Apart from the above, implementation of some events is currently work-in-progress, on which I would continue to work on. For example:

- Line-of-sight event (https://github.com/poliastro/poliastro/pull/1258).
- Satellite visibility with respect to a ground station (https://github.com/poliastro/poliastro/pull/1299)
- Satellite view event (https://github.com/poliastro/poliastro/pull/1298).
- Satellite-satellite collision detection.

## Other work (bug fixes, additional patches)
- We observed some general and events-related bugs, one of which was related to orbit propagation termination during event tracking:
    - Issue: https://github.com/poliastro/poliastro/issues/1285
    - PR: https://github.com/poliastro/poliastro/pull/1288

- Some examples of additional and/or supporting patches include:

    - Marginal newton iteration speed enhancement for eccentric and hyperbolic anomaly calculation (https://github.com/poliastro/poliastro/pull/1247).
    - Adding tests:
        - https://github.com/poliastro/poliastro/pull/1255
        - https://github.com/poliastro/poliastro/pull/1272

- Blog posts:
    - https://www.poliastro.space/blog/2021/06/06/poliastro-gsoc-yash/
    - https://www.poliastro.space/blog/2021/07/19/adding-ale-event-detectors/

## Acknowledgments

I am very grateful to, and sincerely thank my mentors, Juan Luis Cano Rodríguez and Jorge Martínez for critically reviewing pull requests, assisting in making event API decisions, and for help in implementing the event validation cases.

