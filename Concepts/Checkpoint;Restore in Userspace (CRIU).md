---
tags:
  - live-migration
  - linux
---
# Checkpoint;Restore in Userspace (CRIU)

- Can be used to checkpoint in-memory state as a collection of files on disk
- The collection of files can then be restored later on
- CRIU needs to be complemented with a file transfer mechanism such as `rsync` to copy the checkpointed files from source to destination server
- Can be used to implement:
	- Cold migration
	- Pre-copy migration
	- Post-copy migration
	- Hybrid migration

---

## Links

%% Links to related concepts. %%
