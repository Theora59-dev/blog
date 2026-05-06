## CheatSheet FFMPEG
## Conversion Basique
- `ffmpeg -i input.mp4 output.avi` : Conversion simple de format.
- `ffmpeg -i input.mp4 -c:v libx264 -c:a aac output.mp4` : Recodage vidéo H.264 + audio AAC.
- `ffmpeg -i input.mp4 -c copy output.mp4` : Copie flux sans recodage (remuxing rapide).
## Découpage et Taille
- `ffmpeg -i input.mp4 -ss 00:01:30 -t 00:00:30 -c copy output.mp4` : Découpage à partir de 1min30 pour 30s.
- `ffmpeg -i input.mp4 -vf scale=1280:720 output.mp4` : Redimensionnement en 720p.
- `ffmpeg -i input.mp4 -vf crop=1280:720:0:0 output.mp4` : Recadrage (largeur:hauteur:x:y).
## Accélération Graphique
- `ffmpeg -hwaccel cuda -i input.mp4 output.mp4` : Décodage GPU NVIDIA (CUDA).
- `ffmpeg -i input.mp4 -c:v h264_nvenc -preset p4 output.mp4` : Encodage H.264 NVIDIA NVENC.
- `ffmpeg -hwaccel vaapi -i input.mp4 -c:v h264_vaapi output.mp4` : Intel/AMD VAAPI pour Linux.
- `ffmpeg -hwaccel qsv -c:v h264_qsv input.mp4 output.mp4` : Intel Quick Sync Video.
## Audio et Flux
- `ffmpeg -i video.mp4 -i audio.mp3 -c copy -map 0:v:0 -map 1:a:0 output.mp4` : Fusion vidéo + audio.
- `ffmpeg -i input.mp4 -an output.mp4` : Suppression audio (-vn pour vidéo).
- `ffmpeg -i input.mp4 -filter:a "volume=1.5" output.mp4` : Augmenter volume audio x1.5.
## Avancé
- `ffmpeg -i input.mp4 -vf "rotate=90" output.mp4` : Rotation 90°.
- `ffmpeg -loop 1 -i image.jpg -t 10 -c:v libx264 -pix_fmt yuv420p output.mp4` : Image vers vidéo 10s.
- `ffmpeg -i input.mp4 -r 30 output.mp4` : Changer framerate à 30 FPS.
## Filtres Complexes
- `-vf` : Filtres vidéo (ex: `-vf "scale=1920:1080,eq=brightness=0.1"`).
- `-af` : Filtres audio (ex: `-af "highpass=f=200"`).
- `-filter_complex` : Multi-flux (ex: pour GIF ou overlays).

Utilisez `ffmpeg -h encoder=libx264` pour détails encodeur. Testez toujours avec `-t 10` pour prévisualiser.
