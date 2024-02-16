FROM ubuntu:22.04


ENV DEBIAN_FRONTEND noninteractive
ENV LANG ja_JP.UTF-8

ARG UID=1000 # ホストのユーザーIDと一致させる
RUN sed -i.org -e 's|ports.ubuntu.com|jp.archive.ubuntu.com|g' /etc/apt/sources.list \
    && apt-get update && apt-get install -y \
    tzdata \
    locales \
    fonts-noto-cjk \
    dbus-x11 \
    pulseaudio \
    fcitx-mozc \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && ln -s -f /usr/share/zoneinfo/Asia/Tokyo /etc/localtime \
    && dpkg-reconfigure tzdata \
    && locale-gen ja_JP.UTF-8 \
    && useradd -m -u ${UID} user

# Update system
RUN DEBIAN_FRONTEND=noninteractive \
    apt update && \
    apt dist-upgrade -y && \
    apt autoclean -y && \
    apt autoremove -y && \
    rm -rf /var/lib/apt/lists/*

# Add Firefox repository and set package preference
# add-apt-repository step fails if installed with --no-install-recommends
RUN DEBIAN_FRONTEND=noninteractive \
    apt update && \
    apt install -y software-properties-common && \
    add-apt-repository -y ppa:mozillateam/ppa && \
    apt remove -y software-properties-common && \
    apt autoclean -y && \
    apt autoremove -y && \
    rm -rf /var/lib/apt/lists/* && \
    echo 'Package: *' > /etc/apt/preferences.d/mozilla-firefox && \
    echo 'Pin: release o=LP-PPA-mozillateam' >> /etc/apt/preferences.d/mozilla-firefox && \
    echo 'Pin-Priority: 1001' >> /etc/apt/preferences.d/mozilla-firefox && \
    echo 'Unattended-Upgrade::Allowed-Origins:: "LP-PPA-mozillateam:${distro_codename}";' \
       > /etc/apt/apt.conf.d/51unattended-upgrades-firefox

# Install Firefox and xauth
RUN apt update && \
    apt install -y --no-install-recommends firefox xauth && \
    rm -rf /var/lib/apt/lists/*

# Authenticate to display and run FF
CMD xauth nmerge /run/secrets/xauth_list && /usr/bin/firefox
# CMD xauth nmerge /run/secrets/xauth_list
