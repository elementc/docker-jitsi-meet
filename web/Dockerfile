ARG JITSI_REPO=jitsi
FROM ${JITSI_REPO}/base

ADD https://dl.eff.org/certbot-auto /usr/local/bin/

COPY rootfs/ /

RUN \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get install -y cron nginx-extras jitsi-meet-web && \
	apt-dpkg-wrap apt-get -d install -y jitsi-meet-web-config && \
    dpkg -x /var/cache/apt/archives/jitsi-meet-web-config*.deb /tmp/pkg && \
    mv /tmp/pkg/usr/share/jitsi-meet-web-config/config.js /defaults && \
	mv /usr/share/jitsi-meet/interface_config.js /defaults && \
	apt-cleanup && \
	rm -f /etc/nginx/conf.d/default.conf && \
	rm -rf /tmp/pkg /var/cache/apt

RUN \
	chmod a+x /usr/local/bin/certbot-auto && \
	certbot-auto --noninteractive --install-only

COPY spacewalk.png /usr/share/jitsi-meet/images/watermark.png


RUN \
	echo ".welcome .header .header-text-title{visibility: hidden; line-height: 0;}" >> /usr/share/jitsi-meet/css/all.css
RUN \
	echo ".welcome .header .header-text-description{visibility: hidden;  line-height: 0; display: block;}" >> /usr/share/jitsi-meet/css/all.css
RUN \
	echo ".welcome .header .header-text-title::after{content: 'Space-Age Teams Require Space-Age Communication';visibility: visible;display:block;line-height:1;}" >> /usr/share/jitsi-meet/css/all.css
RUN \
	echo ".welcome .header .header-text-description::after{content: 'Effortless, secure, fast, and reliable voice and video calls- whether you\'re in Houston or LEO.';visibility: visible;display:block;line-height:1;}" >> /usr/share/jitsi-meet/css/all.css

RUN \
	echo ".deep-linking-mobile__text{display: none;} a {display:none;} a:nth-of-type(3) {display:block;}" >> /usr/share/jitsi-meet/css/all.css




EXPOSE 80 443

VOLUME ["/config", "/etc/letsencrypt", "/usr/share/jitsi-meet/transcripts"]
