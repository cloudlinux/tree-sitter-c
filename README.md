# tree-sitter-c

[![CI][ci]](https://github.com/tree-sitter/tree-sitter-c/actions/workflows/ci.yml)
[![discord][discord]](https://discord.gg/w7nTvsVJhm)
[![matrix][matrix]](https://matrix.to/#/#tree-sitter-chat:matrix.org)
[![crates][crates]](https://crates.io/crates/tree-sitter-c)
[![npm][npm]](https://www.npmjs.com/package/tree-sitter-c)
[![pypi][pypi]](https://pypi.org/project/tree-sitter-c)

C grammar for [tree-sitter](https://github.com/tree-sitter/tree-sitter).
Adapted from [this C99 grammar](http://slps.github.io/zoo/c/iso-9899-tc3.html).

[ci]: https://img.shields.io/github/actions/workflow/status/tree-sitter/tree-sitter-c/ci.yml?logo=github&label=CI
[discord]: https://img.shields.io/discord/1063097320771698699?logo=discord&label=discord
[matrix]: https://img.shields.io/matrix/tree-sitter-chat%3Amatrix.org?logo=matrix&label=matrix
[npm]: https://img.shields.io/npm/v/tree-sitter-c?logo=npm
[crates]: https://img.shields.io/crates/v/tree-sitter-c?logo=rust
[pypi]: https://img.shields.io/pypi/v/tree-sitter-c?logo=pypi&logoColor=ffd242

## Changes done

* For multiline macro, in-between comments support
```c
#define def_chown_if_needed(chown_function, type_dst)                  \
static int chown_function ## _if_needed (type_dst dst,                 \
                                         const struct stat *statp,     \
                                         uid_t old_uid, uid_t new_uid, \
                                         gid_t old_gid, gid_t new_gid) \
{                                                                      \
	uid_t tmpuid = (uid_t) -1;                                     \
	gid_t tmpgid = (gid_t) -1;                                     \
                                                                       \
	/* Use new_uid if old_uid is set to -1 or if the file was      \
	 * owned by the user. */                                       \
	if (((uid_t) -1 == old_uid) || (statp->st_uid == old_uid)) {   \
		tmpuid = new_uid;                                      \
	}                                                              \
	/* Otherwise, or if new_uid was set to -1, we keep the same    \
	 * owner. */                                                   \
	if ((uid_t) -1 == tmpuid) {                                    \
		tmpuid = statp->st_uid;                                \
	}                                                              \
                                                                       \
	if (((gid_t) -1 == old_gid) || (statp->st_gid == old_gid)) {   \
		tmpgid = new_gid;                                      \
	}                                                              \
	if ((gid_t) -1 == tmpgid) {                                    \
		tmpgid = statp->st_gid;                                \
	}                                                              \
                                                                       \
	return chown_function (dst, tmpuid, tmpgid);                   \
}
```


* Added support for __scanf and __printf gcc attributes `https://gcc.gnu.org/onlinedocs/gcc-3.1.1/gcc/Function-Attributes.html`