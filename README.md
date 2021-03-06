# Config Reader

Go package for reading cofig file by JSON, XML, YAML.

## Installation

```bash
go get github.com/go-trellis/formats/inner-types
go get gopkg.in/yaml.v2
go get github.com/go-trellis/config
```

```bash
go get -u github.com/go-trellis/config
```

### imports

import [gopkg.in/yaml.v2](https://github.com/go-yaml/yaml)

## Usage

### Config

* dot separator to get values, and if return nil, you should set default
* You can do like this: c.GetString("a.b.c") Or c.GetString("a.b.c", "default")
* Supported: .json, .yaml, .yml
* A: ${X.Y.Z} for finding out X.Y.Z's value and setting into A. [See copy example](example/json.go#L23):[See config](example/example.json#L9)

```go
c, e := NewConfig(name)
c.GetString("a.b.c")
```

### Feature

```go
// Config manager data functions
type Config interface {
	// get a object
	GetInterface(key string, defValue ...interface{}) (res interface{})
	// get a string
	GetString(key string, defValue ...string) (res string)
	// get a bool
	GetBoolean(key string, defValue ...bool) (b bool)
	// get a int
	GetInt(key string, defValue ...int) (res int)
	// get a float
	GetFloat(key string, defValue ...float64) (res float64)
	// get list of objects
	GetList(key string) (res []interface{})
	// get list of strings
	GetStringList(key string) []string
	// get list of bools
	GetBooleanList(key string) []bool
	// get list of ints
	GetIntList(key string) []int
	// get list of float64s
	GetFloatList(key string) []float64
	// get time duration by (int)(uint), exp: 1s, 1day
	GetTimeDuration(key string, defValue ...time.Duration) time.Duration
	// get byte size by (int)(uint), exp: 1k, 1m
	GetByteSize(key string) *big.Int
	// get key config
	GetConfig(key string) Config
	// set key's value into config
	SetKeyValue(key string, value interface{}) (err error)
	// get all config
	Dump() (bs []byte, err error)
}
```

### More Example

[See More Example](example)

```bash
go run example/json.go

go run example/yml.go
```


### Reader Repo

```go
// Reader reader repo
type Reader interface {
	// read file into model
	Read(path string, model interface{}) error
	// dump configs' cache
	Dump(model interface{}) ([]byte, error)
}
```

```go
r := New(ReaderType)
if err := r.Read(filename, model); err != nil {
	return
}
```

### Readers

```go
jReader := NewJSONReader()
xReader := NewXMLReader()
yReader := NewYAMLReader()
```

* if you want to judge reader by file's suffix

```go
sReader := NewSuffixReader()
```

* .json = NewJSONReader()
* .xml = NewXMLReader()
* .yaml | .yml = NewYAMLReader()

### TODO

supported Config .xml: now go encoding/xml not supported map[string]interface{}
