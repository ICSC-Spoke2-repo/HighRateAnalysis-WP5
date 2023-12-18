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

COPY bokehjs/bokeh-3.3.1.min.js /usr/local/share/jupyterhub/static/js/bokeh.min.js
COPY bokehjs/bokeh-widgets-3.3.1.min.js /usr/local/share/jupyterhub/static/js/bokeh-widgets.min.js
COPY bokehjs/bokeh-tables-3.3.1.min.js /usr/local/share/jupyterhub/static/js/bokeh-tables.min.js
COPY bokehjs/bokeh-gl-3.3.1.min.js /usr/local/share/jupyterhub/static/js/bokeh-gl.min.js
COPY bokehjs/bokeh-mathjax-3.3.1.min.js /usr/local/share/jupyterhub/static/js/bokeh-mathjax.min.js
COPY bokehjs/bokeh-api-3.3.1.min.js /usr/local/share/jupyterhub/static/js/bokeh-api.min.js

CMD ["/usr/bin/start.sh", "jupyterhub"]
