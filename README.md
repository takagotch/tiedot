### tiedot
---
https://github.com/HouzuoGuo/tiedot

```go
// db/col_test.go



func TestLoadErrorOpenHashTableWhenParseIndex(t *testing.T) {
  defer os.RemoveAll(TEST_DATA_DIR)
  db, _ := OpenDB(TEST_DATA_DIR)
  index := "index_test"
  errMessage := "error OpenHashTable"
  col, _ := OpenCol(db, "test")
  col.Index([]string{index})
  for key, _ := range col.hts {
    col.hts[key] = nil
  }
  patch := monkey.PatchInstanceMethod(reflect.TypeOf(db.Config), "OpenHashTable", func(_ *data.Config, path string) (ht *data.HashTable, err error) {
    if strings.Contains(path, index) {
      return nil, errors.New(errMessage)
    }
    return patch.Unpatch()
  })
  defer patch.Unpatch()
  
  if _, err := OpenCol(db, "test"); err.Error() != errMessage {
    t.Error("Expected error open hash table")
  }
}






```

```
```

```
```

