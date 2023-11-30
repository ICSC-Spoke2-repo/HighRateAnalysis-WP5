FROM ghcr.io/ttedeschi/dodas-iam-client-rec:test-update as REGISTRATION

FROM jupyterhub/k8s-hub:0.11.1

USER root

RUN pip3 install pyjwt

RUN pip3 install kubernetes

RUN curl -k  https://raw.githubusercontent.com/DODAS-TS/dodas-apps/master/logo.png -o /etc/logo.png

RUN echo "jovyan ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers && \
       rm /bin/sh && \
       ln -s /bin/bash /bin/sh

RUN mkdir -p .init

COPY --from=REGISTRATION /usr/local/bin/dodas-IAMClientRec /srv/.init/dodas-IAMClientRec

RUN apt install wget

WORKDIR /usr/local/share/jupyterhub/static/js
RUN wget https://cdn.bokeh.org/bokeh/release/bokeh-3.0.0.min.js
RUN wget https://cdn.bokeh.org/bokeh/release/bokeh-widgets-3.0.0.min.js
RUN wget https://cdn.bokeh.org/bokeh/release/bokeh-tables-3.0.0.min.js
RUN wget https://cdn.bokeh.org/bokeh/release/bokeh-gl-3.0.0.min.js
RUN wget https://cdn.bokeh.org/bokeh/release/bokeh-mathjax-3.0.0.min.js
RUN mv bokeh-3.0.0.min.js -o bokeh.min.js
RUN mv bokeh-widgets-3.0.0.min.js bokeh-widgets.min.js
RUN mv bokeh-tables-3.0.0.min.js bokeh-tables.min.js
RUN mv bokeh-gl-3.0.0.min.js bokeh-api.min.js
RUN mv bokeh-mathjax-3.0.0.min.js bokeh-mathjax.min.js

CMD ["/usr/bin/start.sh", "jupyterhub"]
