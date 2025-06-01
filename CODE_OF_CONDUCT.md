from moviepy.editor import ImageClip
from PIL import Image, ImageDraw, ImageFont

# Paramètres des chemins
original_image_path = "/mnt/data/IMG_F84400C0-38E3-4B6E-8C22-96583B4F1681.jpeg"
image_with_bubble_path = "/mnt/data/image_with_bubble.png"
video_output_path = "/mnt/data/animation_bulle.mp4"

# Charger et modifier l'image (ajout de bulle de dialogue)
image = Image.open(original_image_path).convert("RGBA")
draw = ImageDraw.Draw(image)
font_path = "/usr/share/fonts/truetype/dejavu/DejaVuSans-Bold.ttf"
font = ImageFont.truetype(font_path, 32)

# Texte et bulle
text = "Bonjour, ça va ?"
bubble_w, bubble_h = 360, 80
bubble_x, bubble_y = 20, 20
text_x, text_y = bubble_x + 20, bubble_y + 20

# Dessiner la bulle et le texte
draw.rectangle([bubble_x, bubble_y, bubble_x + bubble_w, bubble_y + bubble_h], fill="white", outline="black", width=3)
draw.text((text_x, text_y), text, font=font, fill="black")

# Sauvegarder l'image temporaire
image.save(image_with_bubble_path)

# Créer animation légère (zoom) pour 2 secondes
clip = ImageClip(image_with_bubble_path).set_duration(2)
# Zoom progressif de 1.0 à 1.05 sur 2 secondes
animated_clip = clip.resize(lambda t: 1 + 0.05 * (t / 2))
animated_clip.write_videofile(video_output_path, fps=24)
