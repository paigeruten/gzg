# gzg

`gzg` helps you keep a folder of text files synced across your devices using a
server you have SSH access to. Each client has a git repo containing the
unencrypted text files. All the server has is the encrypted, gzipped, tar'd,
bare git repo in a single file: `gzg.git.tar.gz.gpg`. When syncing, this file
is pulled down via `scp`, and decrypted and unzipped to a local repository
which is then pulled from and pushed to.

## Usage

Initialize a folder or existing git repo using `gzg init <gpg-id> <remote>`:

    $ mkdir notes
    $ cd notes
    $ vim TODO.txt
    $ gzg init jeremy.ruten@gmail.com jeremy@myserver:git/notes.gzg

Then run the `gzg clone <remote> <folder>` command on each other device:

    $ gzg clone jeremy@myserver:git/notes.gzg notes
    $ cd notes
    $ vim TODO.txt

To sync, run `gzg sync` or simply `gzg`:

    $ gzg sync

## How it works

* `git add -A` is used to automatically make commits when running `init` or
  `sync`.
* The GPG id and remote path are stored in the `gzg` namespace of the git
  repo's `.git/config` file.

## TODO

* More robust code
  * Input validation
  * Error checking
  * Don't use `.git/` as a place to temporarily work with files
  * Don't `scp` back up to the server if there aren't any changes
  * Allow being anywhere in git repo instead of the root when issuing commands
* Combine `init` and `clone` into one command, depending on whether the remote
  path exists?
* Use a checksum to see if the remote file has changed and actually needs to be
  pulled down.
* Better name?

