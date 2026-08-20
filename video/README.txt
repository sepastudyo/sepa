SHOWREEL VIDEO
==============
Files here:

  sepa-showreel.mp4          what ships - 1920x804, 24fps, H.264, ~15 MB
  sepa-showreel-poster.jpg   still from 0:02, cropped to match
  sepa-showreel-master.mp4   296 MB original, 1920x1080 letterboxed. NEVER
                             deployed - .vercelignore excludes video/*-master.*
                             Keep it: it is the source for any re-encode.

This reel opens the site: it is the first section, full-bleed, 100vh.

Behaviour
  - No controls, no play button, no timecode. It runs on its own, muted and
    looping. Muted is not a preference: browsers only allow autoplay without
    sound, and there is no control left to unmute it.
  - Because nothing can ever play the audio, the web encode has NO audio track
    at all (-an). That is ~2 MB every visitor would otherwise download for
    nothing. Restore sound only if you also restore a control for it.
  - It pauses when the section scrolls out of view and resumes on the way back,
    and does not autoplay at all for reduced-motion visitors.
  - The section pins for one screen of scroll while the title fades off it.

The crop
  The master is a 2.39:1 image inside a 1920x1080 frame, with 138px black bars
  top and bottom. The page shows the video full-bleed with object-fit: cover, so
  those bars would have been visible on a 16:9 screen. The web encode therefore
  crops them away to a clean 1920x804. On a 16:9 screen object-fit now trims
  roughly 25% off the left and right instead - frame your shots accordingly.
  Drop the -vf crop line from both commands below to get the bars back.

Re-encoding from the master
  ffmpeg -i video/sepa-showreel-master.mp4 \
    -vf "crop=1920:804:0:138" \
    -c:v libx264 -crf 25 -preset slow -profile:v high -level 4.0 \
    -pix_fmt yuv420p -maxrate 2500k -bufsize 5000k \
    -movflags +faststart -an \
    -y video/sepa-showreel.mp4

  -movflags +faststart is not optional: without it the browser has to download
  the whole file before the first frame plays.
  Lower CRF = better and bigger (23 is near-transparent, 28 is visibly soft).

Poster
  ffmpeg -ss 2 -i video/sepa-showreel-master.mp4 -vf "crop=1920:804:0:138" \
    -frames:v 1 -q:v 3 -y video/sepa-showreel-poster.jpg
