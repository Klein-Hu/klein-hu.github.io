---
layout: post
title: Proto Buffers V3 Spec Reading Notes
categories: [Tips, Library, ProtoBuf]
tags: [Linux Libraries]
fullview: false
comments: true
---

[Proto Buffers](https://developers.google.com/protocol-buffers/) is more and more popular for binary payload. To better understand the definition of Proto Buffers definition, I decided to read the spec directly and get the most important pieces together. Here is the summary of [proto3](https://developers.google.com/protocol-buffers/docs/proto3). I assume readers understand basic concepts, like `message`, `field` and also C/C++ value types, etc.

## Field

**Field Number**

* Unique 
* Should not change after in use 
* 1-15 encoded in 1 byte 
* 16-2047 in 2 byes 
* 19000 - 19999 reserved by Google

**Field Rules** 

* Singular field could shows 0 or 1 time in the message 
* `repeated` could repeat 0 to n times in the message 
* (proto3 only) `repeated` for scalar numeric types using `packed` encoding 
* Number and name could be reserved to avoid re-use of them. 

## Type

**Scalar value types**

* `double`, `float`, `int32`, `int64`, `uint32`, `uint64`, `sint32`, `sint64`, `fixed32`, `fixed64`, `sfixed32`, `sfixed64`, `bool`, `string`, `bytes` 
* Default values: same as C/C++/C#, except message field which language-dependent.

**Enumeration**

* Must have a constant mapping to 0, value should be in range of 32-bit int. Negative is inefficient and not recommended. 
* `option allow_alias = true;` could allow two constants having the same value. 
* Generate `enum` for Java and C++; generate `EnumDescriptor` for Python 
* Unrecognized value handling is language specific 
    * C++: deserialize will keep the unrecognized value.  
    * Python:  
        * Proto v2: unrecognized value will cause exception 
        * Proto v3: any `int32` value will be kept, no exception 
* `reserved` could also be used in `enum` to avoid upgrade conflict

**Using Other Message Types**

* `import` only visible in current `.proto` file; `import public` could be visible to whoever import current file. 
* `protoc` has parameter `-I/--proto_path` to define the search path. If missing, using the directory where the compiler was invoked. 
* Proto3 and proto2 could import and use each other's message except proto2 `enum` could not be used directly in proto3 syntax. 
* Nested type is supported and by default public to others: `Parent.Type`. Could nested multiple layers

**Compatibility**

* `int32`, `uint32`, `int64`, `uint64`, `bool` 
* `sint32`, `sint64` 
* `string`, `bytes`: only when `bytes` are valid UTF-8 
* Embedded message, `bytes` 
* `fixed32`, `sfixed32` 
* `fixed64`, `sfixed64` 
* `enum`, `int32`, `uint32`, `int64`, `uint64`: in terms of wire format. Client code may treat them differently. 
* Changing a single value into a member of a new `oneof` is safe and binary compatible. Moving multiple fields into a new `oneof` may be safe if you are sure that no code sets more than one at a time. Moving any fields into an existing `oneof` is not safe.

**`any`**

To define a field w/o `.proto` definition

* The default type URL for a given message type is `type.googleapis.com/packagename.messagename`. 
* It is a replacement of `extensions` in `proto2`. 
* Currently the runtime libraries for working with `any` types are under development.

**`oneof`**

Think it as `union` in C/C++

* CANNOT use `repeated` field in the Oneof scope. A `Oneof` field could not be `repeated` 
* Reflection APIs work for oneof field. 
* C++ needs to be careful on memory usage and also the `swap()` will change available field as well. Detail is [here](https://developers.google.com/protocol-buffers/docs/proto3#oneof-features) 
* Add/Remove field may cause problem. 

**`map`**

Think it as `unordered_map` in C++

* `map<key_type, value_type> map_field = N;`
* `enum` is NOT valid key_type 
* Could not use `repeated` on map 
* Dup key in merge: last value will be taken 
* Dup key in parsing: error! 

**`package`**

To prevent name clashes between protocol message types. Something like namespace in C++. `option csharp_namespace` could create special C# namespace in a package. 

* first the innermost scope is searched, then the next-innermost, and so on, with each package considered to be "inner" to its parent package. 
* A leading `.` (for example, `.foo.bar.Baz`) means to start from the outermost scope instead.

## Service

* Generate server code stub 
* Don't have to be grpc. Third party plugins: https://github.com/protocolbuffers/protobuf/blob/master/docs/third_party.md 

Sample here: 

```protobuf
service SearchService { 
  rpc Search (SearchRequest) returns (SearchResponse); 
}
```

## JSON Mapping

* 1-1 type mapping to json: https://developers.google.com/protocol-buffers/docs/proto3#json 

* Available Options: 
    * Emit fields with default values 
    * Ignore unknown fields 
    * Use proto field name instead of lowerCamelCase name 
    * Emit enum values as integers instead of strings 

## Options:

* Does not change the meaning but may affect the way it is handled 
* Full list: [`google/protobuf/descriptor.proto`]( https://github.com/protocolbuffers/protobuf/blob/master/src/google/protobuf/descriptor.proto)
* Commonly used: 
    * Java: `java_package` , `java_multiple_files` , `java_outer_classname`  
    * Object-C: `objc_class_prefix` 
    * File:  
        * `optimize_for`: `SPEED`(default), `CODE_SIZE`, or `LITE_RUNTIME`(limited features but good for mobile devices) 
        * `cc_enable_arenas`: improve memory allocation/free performance, **C++ only** 
        * `deprecated`: consider using "reserved" 
    * Custom Options: rarely used 

