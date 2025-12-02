FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive
ENV USER=root

# Install XFCE + TurboVNC + noVNC in one fast layer
RUN apt-get update && apt-get install -y --no-install-recommends \
    xfce4 xfce4-goodies \
    turbovnc \
    novnc \
    websockify \
    firefox \
    && rm -rf /var/lib/apt/lists/*

# Default VNC password = 123456 (change it later inside the desktop)
RUN mkdir -p /root/.vnc && \
    echo "123456" | vncpasswd -f > /root/.vnc/passwd && \
    chmod 600 /root/.vnc/passwd

# Auto-start script (runs every time Codespaces starts)
RUN echo '#!/bin/bash\n\
echo "Starting TurboVNC + noVNC..."\n\
/opt/TurboVNC/bin/vncserver :1 -geometry 1600x900 -depth 24 -securitytypes none\n\
websockify --web=/usr/share/novnc/ 6080 localhost:5901 &\n\
echo "→ Open the Ports tab → click 6080 → you are in!"\n\
tail -f /dev/null' > /start.sh && chmod +x /start.sh

EXPOSE 6080
CMD ["/start.sh"]
