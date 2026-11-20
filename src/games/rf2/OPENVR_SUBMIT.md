# RF2 OpenVR submission correlation, step 2b

The application projection is observed in the common `end_frame()` implementation, directly from
the incoming `XrFrameEndInfo`, before menu injection or any views4/quad rewriting. This is the
actual layer entry point used by RF2/OpenComposite.

The snapshot records:

- monotonically increasing submission sequence;
- left/right eye identity by projection-view order;
- the exact `XrSwapchain` submitted for each eye;
- each eye's `XrRect2Di` bounds;
- whether both eyes share one swapchain or use separate swapchains.

RF2 logs the first eight submissions and then every 300th. This remains observation-only. The
working FP16/MSAA scene backend and focus compositor are unchanged.
