# mavica

aka `copythatfloppy`

This is a translation of the bash script that I use to copy my Mavica FD-7 pictures off the floppy disks to my Linux computer using an USB floppy disk reader.

## Usage

```
mavica /dev/sdc
```

In the simplest form, pass the block device that represents your floppy disk drive. In my system, it's `/dev/sdc`. This will mount the floppy, copy all the pictures from it to the current directory, changing their filenames so they're a bit more unique. For example, a floppy I copied while writing this README had two "fine" quality pictures, the last of which was copied as `2024-09-02_MVC-002F.JPG`. If you're ripping multiple floppies at once, pass the `-n` or `--floppyno` argument with the number of this floppy. That will add a "floppy1" string to the middle of this filename to distinguish it from the others, for example `2024-08-16_floppy1_MVC-006F.JPG`.

You can override the destination directory using `--destination /path/to/dir`, which can be relative or absolute.

By default, the script asks you what date the pictures were taken on. You can hit Enter or Ctrl+C at that prompt to accept the default of today's date, or enter another date in YYYY-MM-DD format.

## Environment Variables

Most of the options for this program can be set in environment variables, so you can rip multiple floppies with ease.

| Variable | Overrides | Description |
| -------- | --------- | ----------- |
| `MVC_DATE` | Date prompt | The date these pictures were taken in YYYY-MM-DD format |
| `MVC_MOUNTPOINT` | `--mountpoint` | Where the floppy should be temporarily mounted during the copy operation |
| `MVC_DSTPATH` | `--destination` | Where the images from this floppy should end up |
| `MVC_FLOPPYNO` | `--floppyno` | The current number of this floppy in a batch |

You can use these by either exporting them into the environment, or running them inline like `MVC_DATE=2024-09-02 mavica -n 5`. Setting `MVC_DATE` will skip the prompt for what date the images were taken on, so you can use these environment variables to run the script without any interactivity.
