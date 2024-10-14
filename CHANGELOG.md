# Change log

# version 2.0.0

- Changed installation method because `k0s` no longer works with an empty join token file.
- Now uses the `k0s controller install` and `k0s worker install` commands.
- On all but the genesis node, the install is done twice.
    - First with a join token file
    - Stopping the service
    - Removing the service
    - Then without the join token file

# version 1.0.1

- Removed the no longer supported `get_md5: false` from `stat` tasks.

# version 1.0.0

- The initial release
