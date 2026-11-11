# Core Java

## Field declarations

| Modifier | Same class | Same package | Subclass (different package) | Other packages |
|---|---|---|---|---|
| `private` | Yes | No | No | No |
| *(no modifier)* package-private | Yes | Yes | No | No |
| `protected` | Yes | Yes | Yes* | No |
| `public` | Yes | Yes | Yes | Yes |

| Modifier | Meaning |
|---|---|
| `static` | Belongs to the class rather than an instance |
| `final` | Can only be assigned once |
| `transient` | Excluded from default serialization |
| `volatile` | Guarantees visibility and ordering of reads/writes across threads |
