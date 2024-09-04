# mavica

The `mavica` script copies all photos off a Mavica camera's floppy disk, elegantly.

Do you have a Sony Mavica camera and a Linux computer you use to copy the pictures off its floppy disks? This script automates that process! It's a pure python, no dependency script that copies all the images from a Mavica floppy disk, formatting their filenames and keeping track of the disk number.

## Help Wanted

This script will probably work for other models of Mavica cameras, and will probably work on other operating systems! However, it's only been developed and tested on Ubuntu Linux with a Mavica FD-7. **If you're using a different operating system or camera model, please try the script out!** Please file an issue to let me know if you hit any problems or report a new working model.

## Usage

```
mavica /dev/sdc
```

In the simplest form, run the `mavica` script with the block device that corresponds with your floppy disk drive. In my system, it's `/dev/sdc`. If you're not sure which device is your floppy drive, `dmesg` output will include the path to your drive when it was plugged in (or when the system booted). If it's a removable drive (like a USB floppy reader), you can run `dmesg -w` before plugging it in to see the log lines update live when it's first attached.

When you run this command, the script will mount the floppy, copy all the pictures from it to the current directory, and improve their filenames. **Mounting the floppy requires root privileges, which the script requests for the mount commands using `sudo`.** No other parts of the script require root privileges and all operate as your current user.

The filename "improvement" makes each filename unique, to avoid the frustration of accidentally overwriting older pictures on copy. For example, a floppy I copied while writing this README had two "fine" quality pictures, the last of which was copied as `2024-09-02_MVC-002F.JPG`.

If you're ripping multiple floppies at once, pass the `-n` or `--floppyno` argument with the number of this floppy. That will add a "floppy1" string to the middle of this filename to distinguish it from the others, for example `2024-08-16_floppy1_MVC-006F.JPG`.

You can set where the pictures should be copied to using `--destination /path/to/dir`, which can be relative or absolute.

By default, the script assumes the pictures were taken on the present day. To override this assumption, use `--date 2024-09-02` or the environment variable `MVC_DATE` (see below). Here's a more involved example:

```
mavica -n 5 --date 2024-07-17 --destination Pictures/mavica/Architecture/ /dev/sdc
```

This copies the fifth floppy of a series (`-n`) to the "Architecture" directory at the path above (`--destination`). The photos were taken on July 17th 2024 (`--date`).

## Environment Variables

Most of the options for this program can be set in environment variables, so you can rip multiple floppies with ease.

| Variable | Overrides | Description |
| -------- | --------- | ----------- |
| `MVC_DATE` | `--date` | The date these pictures were taken in YYYY-MM-DD format |
| `MVC_MOUNTPOINT` | `--mountpoint` | Where the floppy should be temporarily mounted during the copy operation |
| `MVC_DSTPATH` | `--destination` | Where the images from this floppy should end up |
| `MVC_FLOPPYNO` | `--floppyno` | The current number of this floppy in a batch |

You can use these by either exporting them into the environment, or running them inline. The above example, reworked to use environment variables, would look like this:

```
MVC_FLOPPYNO=5 MVC_DATE=2024-07-17 MVC_DSTPATH=Pictures/mavica/Architecture/ mavica /dev/sdc
```
