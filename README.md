### tiedot
---
https://github.com/HouzuoGuo/tiedot

```go
// db/col_test.go
package db

import (
  "encoding/json"
  "io/ioutil"
  "os"
  "reflect"
  "strings"
  "testing"
  
  "bou.ke/monkey"
  "github.com/HouzuoGuo/tiedot/data"
  "github.com/pkg/errors"
)

func TestColMkDirErr(t *testing.T) {
  db, _ := OpenDB(TEST_DATA_DIR)
  errMessage := "Error mak dir"
  patch := monkey.Patch(os.MkdirAll, func(path string, perm os.File))
  defer patch.Unpatch()
  _, err := openCol(db, "test")
  if err.Error() != errMessage {
    t.Error("Expected error message make dir")
  }
}

func TestOpenPartitionErr(t *testing.T) {

}

func TestOpenReadDirErr(t *testing.T) {

}

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

func TestClose(t *testing.T) {

}

func TestInde 



```

```
```

```
```

