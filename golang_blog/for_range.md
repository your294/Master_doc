# 坑
```go
var arr_p []*int
var arr = []int {1, 2, 3}
// right
for i := range arr {
    arr_p = append(arr_p, &arr[i])
}
// error
for i, v := range arr {
    arr_p = append(arr_p, &v)
}
```
# after compile
```go
ha := a
hv1 := 0
hn := len(ha)
v1 := hv1
v2 := nil // Note: this v2 has a memory and when you choose the v2 with its address 
for ; hv1 < hn; hv1++ {
    tmp := ha[hv1]
    v1, v2 = hv1, tmp
    ...
}

```