# HPE OneView golang bindings

[![Build Status](https://travis-ci.org/Pooja-S-Rao/oneview-golang.svg?branch=master)](https://travis-ci.org/Pooja-S-Rao/oneview-golang)

HPE OneView allows you to treat your physical infrastructure as code, and now
you can integrate your favorite tools based in golang with HPE OneView.

## Build requirements
We use docker to build and test, run this project on a system that has docker. 
If you don't use docker, you will need to install and setup go-lang locally as
well as any other make requirements.  You can use `USE_CONTAINER=false` environment
setting for make to avoid using docker. Otherwise make sure to have these tools:
- docker client and daemon
- gnu make tools

## Testing your changes

### From a container
```
make test
```

### Without docker
* Install golang 1.5 or better
* Install go packages listed in .travis.yml
```
USE_CONTAINER=false make test
```

## Contributing

Want to hack on oneview-golang? Please start with the [Contributing Guide](https://github.com/Pooja-S-Rao/docker-machine-oneview/blob/master/CONTRIBUTING.md).

This code base is primarily consumed and used by the docker-machine-oneview project.  We will follow contribution standards set by this project.

## Hacking Guide

Use the [hacking guide](HACKING.md) to run local acceptance testing and debugging local contributions.
Currently test cases are not supported portion of our CI/CD approval process but might be made available from this test suite in the future.   Contributions to the test suite is appreciated.

## License
This project is licensed under the Apache License, Version 2.0.  See LICENSE for full license text.


## Changes to support HPE OneView API Version 201 and 300 :
1. In the /oneview-golang/liboneview folder inside the file api_support.go
  func (o APISupport) IsSupported(v Version) bool {
    switch o {
      case C_SERVER_HARDWAREV2:
      return (API_VER2 == v || API_VER2_1 == v || API_VER3 == v)      #To return true for the Server hardware of OneView API version
                                                                       201 and 300
      case C_PROFILE_TEMPLATES:
      return (API_VER2 == v || API_VER2_1 == v || API_VER3 == v) )    #To return true for the Profile template of OneView API version
                                                                       201 and 300
      default:
      return true
    }
  }
2.In the /oneview-golang/liboneview folder inside the file version.go
  a) # const (
    Ver1 = 228 // OV api version (120) + ICSP api version (108)
    Ver2 = 308 // OV api version (200) + ICSP api version (108)
    Ver2_1 = 309 // OV api version (201) + ICSP api version (108)    #Added to support the API Version 201 of OneView
    Ver3 = 408 // OV api version (300) + ICSP api version (108)      #Added to support the API Version 300 of OneView
      
    Unknown = -1
  )
  b) // Supported versions
    const (
      API_VER1 Version = Ver1
      API_VER2 Version = Ver2
      API_VER2_1 Version = Ver2_1       #Added to support the API Version 201 of OneView
      API_VER3 Version = Ver3           #Added to support the API Version 300 of OneView
      API_VER_UNKNOWN Version = Unknown
    )
  c) // verstringlist - String list description of supported drivers
    var verstringlist = [...]string{
      "HP OneView 120,HP ICSP 108",
      "HP OneView 200,HP ICSP 108",
      "HP OneView 201,HP ICSP 108",     #Added to Support the drivers of API Version of 201 of OneView
      “HP OneView 300,HP ICSP 108”,     #Added to Support the drivers of API Version of 300 of OneView
      "Unknown",
    }
  d) // verintlist - Integer list description of supported drivers
    var verintlist = [...]int{
      Ver1,
      Ver2,
      Ver2_1,   #Added to Support the drivers of API Version of 201 of OneView
      Ver3,     #Added to Support the drivers of API Version of 300 of OneView
      Unknown,
    }
  e) #// init the version mapping
    func init() {
      validVersion = map[int]bool{
      Ver1: true,
      Ver2: true,
      Ver2_1:true,   #Added to map a valid version
      Ver3: true,    #Added to map a valid version
      }
    }
3.In the /oneview-golang/ov folder inside the file profiles.go
 func (c *OVClient) CreateProfileFromTemplate(name string, template ServerProfile, blade ServerHardware) error {
  a) ovversion = c.APIVersion   #To retrieve the API Version of OneView
  //GET on /rest/server-profile-templates/{id}new-profile
    if c.IsProfileTemplates() {
    log.Debugf("getting profile by URI %+v, v2", template.URI)
    new_template, err = c.GetProfileByURI(template.URI)
    if err != nil {
      return err
  }
  b) if ovversion >= 300 {   #If the OneView API Version is > = 300 then Server Profile type is "ServerProfileV6”
    new_template.Type = "ServerProfileV6”
    } else {    #If the OneView API Version is less than 300 then Server Profile type is "ServerProfileV5"
      new_template.Type = "ServerProfileV5"
    } 
  c) //new_template.Type = "ServerProfileV5"   #The line number 284 in profiles.go has to remove because we have included in the if condition
