### What happens and why?

- `i++` and `i--` do 3 steps (read, modify, write).
- Two goroutines both update `i` as "fast as they can"
- They read the same value, modify it separately, overwrite each other.
This makes the value of i unpredictable.

### Fix

- Use `sync.Mutex`
- Or use `channels`
