# 6. IPv6 Compression Rules

IPv6 addresses are very long and difficult to remember.

Therefore, IPv6 provides several rules to shorten addresses without changing their meaning.

---

## Rule 1: Remove Leading Zeros

Leading zeros inside a block can be omitted.

Example:

Original:

```text
2001:0db8:0000:0001
```

Compressed:

```text
2001:db8:0:1
```

---

## Rule 2: Replace Consecutive Zero Blocks with ::

Continuous blocks containing only zeros can be replaced with:

```text
::
```

Example:

Original:

```text
2001:db8:0000:0000:0000:0000:0000:0001
```

Compressed:

```text
2001:db8::1
```

---

## Rule 3: :: Can Be Used Only Once

Using `::` multiple times creates ambiguity because the number of omitted blocks becomes unknown.

Incorrect:

```text
2001::abcd::1
```

Correct:

```text
2001:0:0:abcd::1
```

---

## Rule 4: Entire Zero Block Becomes 0

If an entire block is:

```text
0000
```

it becomes:

```text
0
```

Example:

Original:

```text
2001:0db8:0000:0000:abcd:0000:0000:0001
```

After removing leading zeros:

```text
2001:db8:0:0:abcd:0:0:1
```

---

## Rule 5: Compress the Longest Sequence of Zero Blocks

If multiple zero sequences exist, compress the longest sequence.

Example:

Original:

```text
2001:0000:0000:abcd:0000:0000:0000:1234
```

Removing leading zeros:

```text
2001:0:0:abcd:0:0:0:1234
```

Longest sequence:

```text
0:0:0
```

Compressed Address:

```text
2001:0:0:abcd::1234
```

---

## Rule 6: If Two Zero Sequences Have Equal Length, Compress the Leftmost Sequence

Example:

Original:

```text
2001:0000:0000:abcd:0000:0000:1234:5678
```

After removing leading zeros:

```text
2001:0:0:abcd:0:0:1234:5678
```

Both zero sequences have equal length.

Therefore, compress the leftmost sequence:

```text
2001::abcd:0:0:1234:5678
```

---

## Step-by-Step Example 1

Original Address:

```text
2001:0db8:0000:0000:0000:8a2e:0370:7334
```

### Step 1: Remove Leading Zeros

```text
2001:db8:0:0:0:8a2e:370:7334
```

### Step 2: Replace Longest Zero Sequence

```text
2001:db8::8a2e:370:7334
```

Final Compressed Address:

```text
2001:db8::8a2e:370:7334
```

---

## Step-by-Step Example 2

Original Address:

```text
fe80:0000:0000:0000:0202:b3ff:fe1e:8329
```

### Step 1

Remove leading zeros:

```text
fe80:0:0:0:202:b3ff:fe1e:8329
```

### Step 2

Compress longest sequence:

```text
fe80::202:b3ff:fe1e:8329
```

Final Address:

```text
fe80::202:b3ff:fe1e:8329
```

---

## Step-by-Step Example 3

Original Address:

```text
2001:0000:0000:abcd:0000:0000:0000:1234
```

### Step 1

Remove leading zeros:

```text
2001:0:0:abcd:0:0:0:1234
```

### Step 2

Find longest zero sequence:

```text
0:0:0
```

Final Address:

```text
2001:0:0:abcd::1234
```

---

## Quick Revision

| Rule   | Description                                                 |
| ------ | ----------------------------------------------------------- |
| Rule 1 | Remove leading zeros                                        |
| Rule 2 | Replace consecutive zero blocks with ::                     |
| Rule 3 | :: can be used only once                                    |
| Rule 4 | 0000 becomes 0                                              |
| Rule 5 | Compress longest zero sequence                              |
| Rule 6 | If equal length sequences exist, compress leftmost sequence |

---

## Memory Trick

Follow these steps:

```text
Remove Leading Zeros
        ↓
Convert 0000 → 0
        ↓
Find Longest Zero Sequence
        ↓
Replace with ::
        ↓
Use :: Only Once
        ↓
If Equal Length Exists,
Compress Leftmost Sequence
```

---

## Biggest Concept

```text
IPv6 compression does not change
the address value.

It only provides a shorter and
easier-to-read representation.
```
End of IPv6 Address Compression Notes