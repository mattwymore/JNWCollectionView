#examine reuse cycle

Either I don't fully understand it or it's wrong somehow. I guess what I mean is: it appears that cells are put into the queue with no further information, so when they're removed, we have no choice but to prepare them for reuse. But my hunch is that cells **shouldn't** be prepared for reuse until they are being provided for a new indexPath.

#perform batch updates

There's a branch for this task. the idea here is to be able to animate changes on a cell- or section-level (inserting, moving and deleting them).

ok, how to implement these insert/move/delete updates? and how to do it in a way that makes batching available?

The meat here is about layout attributes. We need to get layout attributes before and after the change, then animate those values.

`performBatchUpdates:` can set a flag `_performingBatchUpdate=YES`, that way the updates block can include calls to the 3 update methods.

`performBatchUpdates` will also set up a temporary `finalAttributes` object that contains the attributes for all cells. I need to make sure that cells moving in from off-screen get drawn properly. But basically each of the 3 basic updates methods will modify `finalAttributes`. At the end of the updates, the animation is triggered between the current layout attributes and the final ones.
s