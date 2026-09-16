# indrajala-datasets-mnist

The MNIST handwritten digit database, packaged as Parquet, for use by
[`perceptron`](https://github.com/davidbarkhuizen/first-principles-networks) and other consumers
that want a pinned, checksum-verified source rather than a manually-supplied local copy.

This repo is a documented *source*, not a preprocessed cache: the data is stored in the same
canonical Parquet format a consuming project already reads, unchanged.

## contents

```
metadata.json                          - repo-level metadata (provenance, format, file list)
data/
  mnist-train.parquet                  - 60000 rows
  mnist-train.parquet.metadata.json    - checksum, record count, schema
  mnist-test.parquet                   - 10000 rows
  mnist-test.parquet.metadata.json     - checksum, record count, schema
```

Each Parquet file's `image` column holds an 8-bit grayscale PNG (28x28) per row, in HuggingFace's
standard image-dataset layout (`image: struct<bytes: binary, path: string>`), alongside an int64
`label` column (0-9). See each file's own `.metadata.json` for the exact schema and a SHA-256
checksum.

## quick start

```python
import pyarrow.parquet as pq
table = pq.read_table("data/mnist-train.parquet")
```

## provenance

See `metadata.json`'s `source` field. The dataset's own license/canonical source URL is currently
unverified (marked `TODO` there) - verify before relying on this for anything beyond the same
personal/research use `perceptron` itself makes of it.

## versioning

Consumers should pin to a specific tag or commit, not `main` - see `metadata.json`'s `version`
field for this repo's own packaging version.

## license

This repo's own packaging (this README, `metadata.json`, directory layout) is MIT-licensed - see
`LICENSE`. The dataset content itself is subject to its own upstream terms; see "provenance" above.
