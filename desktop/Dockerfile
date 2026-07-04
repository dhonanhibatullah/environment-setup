FROM kasmweb/ubuntu-noble-desktop:x86_64-1.19.0

USER root

RUN apt-get update \
    && apt-get install -y --no-install-recommends sudo \
    && echo "kasm-user ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/kasm-user \
    && chmod 0440 /etc/sudoers.d/kasm-user \
    && rm -rf /var/lib/apt/lists/*

USER kasm-user
