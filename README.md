# NFJFin
Netflix inspired Jellyfin Theme

# Login Page
<img width="1920" height="1080" alt="Login_Page" src="https://github.com/user-attachments/assets/ff62c25f-6fee-4cc2-9e0e-b5c3092296ec" />

# Forgot Password Page
<img width="1920" height="1080" alt="Forgot_Password_Page" src="https://github.com/user-attachments/assets/4508c661-5d89-4c39-90d8-67ecd461aa45" />

# Collections Page with Gray Triangular Indicator on Top Right of the Poster
<img width="1920" height="1080" alt="Collection" src="https://github.com/user-attachments/assets/1f510ca8-2360-4d17-8338-2df7392b72e8" />

# Version Indicator as a Red Triangule on the Top Left of the Poster
<img width="1920" height="1080" alt="Version_Page" src="https://github.com/user-attachments/assets/bf9fa79b-819c-47e4-baef-a1a4a669951b" />

# For Background and Fonts
Copy the "img" and "fonts" folder to the assets directory of jellyfin install location.

# Implimentation and Edit
Copy the CSS code from the CSS.txt file to Dashboard -> Branding -> Custom CSS Code. <br> You are free to edit the project.

# For Docker Installation
This instructions are generated using AI<br>
To implement the NFJFin theme on a Docker-hosted Jellyfin server, the core challenge is making the local repository's asset files (img and fonts folders) accessible to the web server running inside the container.<br>Add this dedicated Docker section to your guide to show users exactly how to map these directories.<br>Docker Installation Guide When running Jellyfin inside a Docker container, the default web asset files are stored isolated inside the container file system at /jellyfin/jellyfin-web/ (or /usr/share/jellyfin/web/ depending on the image version).<br>You must mount your local theme directories directly into these target paths<br>Option 1: Using Docker Compose (Recommended)Add the local img and fonts asset folder locations as bind mounts under the volumes section of your docker-compose.yml file:yamlversion: "3.5"
<br>services:
  jellyfin:
    image: jellyfin/jellyfin:latest
    container_name: jellyfin
    network_mode: host
    volumes:
      - /path/to/config:/config
      - /path/to/cache:/cache
      - /path/to/media:/media
      # --- NFJFin Theme Bind Mounts ---
      - ./img:/jellyfin/jellyfin-web/assets/img
      - ./fonts:/jellyfin/jellyfin-web/assets/fonts
    restart: unless-stopped
Use code with caution.<br>Option 2: Using the Docker CLIIf you deploy your container using a docker run statement, append the two asset folder parameters to your command:bashdocker run -d \
 --name jellyfin \
 -p 8096:8096 \
 --volume /path/to/config:/config \
 --volume /path/to/cache:/cache \
 --mount type=bind,source=/path/to/media,target=/media \
 --mount type=bind,source=/absolute/path/to/NFJFin/img,target=/jellyfin/jellyfin-web/assets/img \
 --mount type=bind,source=/absolute/path/to/NFJFin/fonts,target=/jellyfin/jellyfin-web/assets/fonts \
 --restart=unless-stopped \
 jellyfin/jellyfin
Use code with caution.💡<br>Note for LinuxServer.io Image Users: If you use the linuxserver/jellyfin image rather than the official jellyfin/jellyfin image, change the target paths inside the container from /jellyfin/jellyfin-web/assets/... to /usr/share/jellyfin/web/assets/....<br>Final Step: Apply Admin Custom CSSRe-deploy or restart your container (docker compose up -d --force-recreate).<br>Log into the Jellyfin web dashboard as an administrator.Navigate to Dashboard → Branding and paste the content from your CSS.txt file into the Custom CSS Code field.Clear your browser cache and refresh the tab to load the new style.
