# mavica

The `mavica` script copies all photos off a Mavica camera's floppy disk, elegantly.

Do you have a Sony Mavica camera and a Linux computer you use to copy the pictures off its floppy disks? This script automates that process! It's a pure python, no dependency script that copies all the images from a Mavica floppy disk, formatting their filenames and keeping track of the disk number.

So far it's only been tested with a Mavica FD-7; I am eager for more testers with other models! Please give the script a try and open an issue or PR with your experience.

## Usage

```
mavica /dev/sdc
```

In the simplest form, pass the block device that represents your floppy disk drive. In my system, it's `/dev/sdc`. This will mount the floppy, copy all the pictures from it to the current directory, changing their filenames so they're a bit more unique. For example, a floppy I copied while writing this README had two "fine" quality pictures, the last of which was copied as `2024-09-02_MVC-002F.JPG`. If you're ripping multiple floppies at once, pass the `-n` or `--floppyno` argument with the number of this floppy. That will add a "floppy1" string to the middle of this filename to distinguish it from the others, for example `2024-08-16_floppy1_MVC-006F.JPG`.

You can override the destination directory using `--destination /path/to/dir`, which can be relative or absolute.

By default, the script assumes the pictures were taken on the present day. To override this assumption, use `--date 2024-09-02` or the environment variable `MVC_DATE`.

## Environment Variables

Most of the options for this program can be set in environment variables, so you can rip multiple floppies with ease.

| Variable | Overrides | Description |
| -------- | --------- | ----------- |
| `MVC_DATE` | `--date` | The date these pictures were taken in YYYY-MM-DD format |
| `MVC_MOUNTPOINT` | `--mountpoint` | Where the floppy should be temporarily mounted during the copy operation |
| `MVC_DSTPATH` | `--destination` | Where the images from this floppy should end up |
| `MVC_FLOPPYNO` | `--floppyno` | The current number of this floppy in a batch |

You can use these by either exporting them into the environment, or running them inline like `MVC_DATE=2024-09-02 mavica -n 5`.
