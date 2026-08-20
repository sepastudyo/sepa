SHOWREEL VIDEO
==============
Files here:

  sepa-showreel.mp4          what ships - 1920x804, 24fps, H.264 + AAC, ~17 MB
  sepa-showreel-poster.jpg   still from 0:02, cropped to match

The 296 MB master is NOT in this folder - it lives at
C:\Users\User\Desktop\sepa-showreel.mp4 (1920x1080, letterboxed, with audio).
Keep it: it is the source for every re-encode. Never put it back in the project,
GitHub rejects files over 100 MB.

This reel opens the site: it is the first section, full-bleed, 100vh.

Behaviour
  - It starts on its own, looping, and tries to start WITH sound. Browsers only
    guarantee autoplay for silent video, so if the attempt is refused the reel
    plays silently and unmutes itself on the visitor's first click, tap or key
    anywhere on the page. Returning visitors often get audio from frame one.
  - The round control in the bottom-right corner switches the sound either way,
    and once it is used the page stops deciding on the visitor's behalf.
  - That control also starts playback for reduced-motion visitors, who get the
    still poster instead of an autoplaying reel.
  - It pauses when the section scrolls out of view and resumes on the way back.
  - No pin: the section scrolls away like any other, so the wheel always moves
    the page. The title just fades off the video on the way out.

The crop
  The master is a 2.39:1 image inside a 1920x1080 frame, with 138px black bars
  top and bottom. The web encode crops those away to a clean 1920x804.

  On screen the page uses object-fit: contain, so the WHOLE frame is always
  visible and the leftover height shows as the section black above and below -
  about 8% on a wide desktop, 14% on a 16:10 laptop. Nothing is ever cut off.
  Cover was tried first but it had to zoom in to fill, and on a 16:10 laptop it
  ate 27% of every shot.

  Below a 4:3 viewport (phones upright) it flips back to cover, because
  containing a 2.39:1 reel on a phone leaves a thin strip.

Re-encoding from the master
  ffmpeg -i "C:\Users\User\Desktop\sepa-showreel.mp4" \
    -vf "crop=1920:804:0:138" \
    -c:v libx264 -crf 25 -preset slow -profile:v high -level 4.0 \
    -pix_fmt yuv420p -maxrate 2500k -bufsize 5000k \
    -c:a aac -b:a 128k -ac 2 \
    -movflags +faststart \
    -y video/sepa-showreel.mp4

  -movflags +faststart is not optional: without it the browser has to download
  the whole file before the first frame plays.
  Lower CRF = better and bigger (23 is near-transparent, 28 is visibly soft).

Poster
  ffmpeg -ss 2 -i "C:\Users\User\Desktop\sepa-showreel.mp4" \
    -vf "crop=1920:804:0:138" -frames:v 1 -q:v 3 \
    -y video/sepa-showreel-poster.jpg
