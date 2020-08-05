# Go client Library for VideoAPI.cloud

## Install

```console
go get github.com/videoapicloud/videoapicloudgo
```

## Submitting the job

Example of `videoapicloud.conf`:

```ini
var s3 = s3://accesskey:secretkey@mybucket

set webhook = http://mysite.com/webhook/videoapicloud

-> mp4  = $s3/videos/video.mp4
-> webm = $s3/videos/video.webm
-> jpg:300x = $s3/previews/thumbs_#num#.jpg, number=3
```

Here is the Go code to submit the config file:

```go
package main

import (
  "fmt"
  "github.com/videoapicloud/videoapicloudgo"
)

func main() {
  config := videoapicloud.Config{
    Conf: "videoapicloud.conf",
    Source: "http://yoursite.com/media/video.mp4",
    Vars: videoapicloud.Vars{"vid": "1234"},
  }

  if job, err := videoapicloud.NewJob(config, "api-key"); err != nil {
    fmt.Println("Error:", err)
  } else {
    fmt.Println("Job created:", job.Id)
  }
}
```

You can also create a job without a config file. To do that you will need to give every settings in the method parameters. Here is the exact same job but without a config file:

```go
vid := "1234"
s3 := "s3://accesskey:secretkey@mybucket"

config := videoapicloud.Config{
  Vars: videoapicloud.Vars{
    "vid": vid,
    "s3": s3,
  },
  Source: "http://yoursite.com/media/video.mp4",
  Webhook: "http://mysite.com/webhook/videoapicloud?videoId=$vid",
  Outputs: videoapicloud.Outputs{
    "mp4": "$s3/videos/video_$vid.mp4",
    "webm": "$s3/videos/video_$vid.webm",
    "jpg:300x": "$s3/previews/thumbs_#num#.jpg, number=3",
  }
}

if job, err := videoapicloud.NewJob(config, "api-key"); err != nil {
  fmt.Println("Error:", err)
} else {
  fmt.Println("Job created:", job.Id)
}
```

Note that you can use the environment variable `VIDEOAPICLOUD_API_KEY` to set your API key.

*Released under the [MIT license](http://www.opensource.org/licenses/mit-license.php).*

---

* VideoAPI.cloud website: https://videoapi.cloud
* API documentation: https://videoapi.cloud/docs
* Contact: [support@videoapi.cloud](mailto:support@videoapi.cloud)
