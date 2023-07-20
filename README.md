
![goinsta logo](https://raw.githubusercontent.com/Davincible/goinsta/v1/resources/goinsta-image.png)

[![GoDoc](https://godoc.org/github.com/blacheinc/goinsta?status.svg)](https://godoc.org/github.com/blacheinc/goinsta) [![Go Report Card](https://goreportcard.com/badge/github.com/blacheinc/goinsta)](https://goreportcard.com/report/github.com/blacheinc/goinsta)

## Go Instagram Private API

> Unofficial Instagram API for Golang

This repository is an extended fork of [ahmdrz/goinsta](https://github.com/ahmdrz/goinsta).

If you are missing anything or something is not working as expected please let
me know through the issues or discussions.

### Features

* **HTTP2 by default. Goinsta uses HTTP2 client enhancing performance.**
* **Object independency. Can handle multiple instagram accounts.**
* **Like Instagram mobile application**. Goinsta is very similar to Instagram official application.
* **Simple**. Goinsta is made by lazy programmers!
* **Backup methods**. You can use Export`and Import`functions.
* **Security**. Your password is only required to login. After login your password is deleted.
* ~~**No External Dependencies**. GoInsta will not use any Go packages outside of the standard library.~~ goinsta now uses [chromedp](https://github.com/chromedp/chromedp) as headless browser driver to solve challanges and checkpoints.

### Package installation 

`go get -u github.com/blacheinc/goinsta`

### Login and export/import cookie as string

```go
package main

import (
	"fmt"

	"github.com/blacheinc/goinsta"
)

func main() {  
  insta := goinsta.New("USERNAME", "PASSWORD")

   // Only call Login the first time you login. Next time import your config
	err := insta.Login()
	if err != nil {
		log.Fatal(err)
	}


// Export your configuration as string
  // after exporting you can use ImportPathString function instead of New function.
  // insta, err := goinsta.ImportPathString("cookie")
  // it's useful when you want use goinsta repeatedly.
	cookie, err := insta.ExportAsString()
	if err != nil {
	   log.Fatal(err)
	}
  // cookie can be saved and encrypted in a database 
}
```

### Get comments on a post

```go
package main

import (
	"fmt"

	"github.com/blacheinc/goinsta"
)

func main() {  
  // import cookie 
  insta, err := goinsta.ImportPathString("cookie")
  if err != nil {
		log.Fatal(err)
	}

  //retrieve postid from url
	postId, err := GetPostId(url)
	if err != nil {
		log.Fatal(err)
	}
	mediaId, err := goinsta.MediaIDFromShortID(postId)
	if err != nil {
		log.Fatal(err)
	}
	//fetch all media
	media, err := insta.GetMedia(mediaId)
	if err != nil {
		log.Fatal(err)
	}
  // load comment 
  comment := media.Items[0].LoadComment()
	if err != nil {
		log.Fatal(err)
	}
  // paginate to populate the comment
  for comment.Next() {
    // loop through to get all commments in a post
		for _, comment := range comment.Comments {
        // Print all users comments 
				fmt.Println(comment.Text)
			}
		}
}
```

`go get -u github.com/blacheinc/goinsta@latest`

### Example

```go
package main

import (
	"fmt"

	"github.com/blacheinc/goinsta"
)

func main() {  
  insta := goinsta.New("USERNAME", "PASSWORD")
  
  // Only call Login the first time you login. Next time import your config
  if err := insta.Login(); err != nil {
          panic(err)
  }

  // Export your configuration
  // after exporting you can use Import function instead of New function.
  // insta, err := goinsta.Import("~/.goinsta")
  // it's useful when you want use goinsta repeatedly.
  // Export is deffered because every run insta should be exported at the end of the run
  //   as the header cookies change constantly.
  defer insta.Export("~/.goinsta")

  ...
}
```

For the full documentation, check the [wiki](https://github.com/Davincible/goinsta/wiki/01.-Getting-Started), or run `go doc -all`.

### Legal

This code is in no way affiliated with, authorized, maintained, sponsored or endorsed by Instagram or any of its affiliates or subsidiaries. This is an independent and unofficial API. Use at your own risk.

