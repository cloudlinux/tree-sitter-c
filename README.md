# tree-sitter-c

[![CI](https://github.com/tree-sitter/tree-sitter-c/actions/workflows/ci.yml/badge.svg)](https://github.com/tree-sitter/tree-sitter-c/actions/workflows/ci.yml)
[![Discord](https://img.shields.io/discord/1063097320771698699?logo=discord)](https://discord.gg/w7nTvsVJhm)
[![Rust Crate](https://img.shields.io/crates/v/tree-sitter-c.svg)](https://crates.io/crates/tree-sitter-c)
[![Node Package](https://img.shields.io/npm/v/tree-sitter-c.svg)](https://www.npmjs.com/package/tree-sitter-c)

C grammar for [tree-sitter](https://github.com/tree-sitter/tree-sitter).
Adapted from [this C99 grammar](http://slps.github.io/zoo/c/iso-9899-tc3.html).

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