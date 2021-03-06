This repo contains FreeSWITCH configuration
for a simple personal SIP to PSTN relay.

# Vars

Variables in `vars.xml` must define
global variables for
switch-level RTP-port range,
inbound/outbound SIP profile ports
and inbound verto profile port.


# Profiles

The config contains
three SIP profiles/interfaces in `configuration/sofia.conf.d/`,
two inbound (one for each of IPv4 and IPv6)
and an IPv4 outbound.

The config contains
two inbound verto profiles/interfaces in `configuration/verto.conf.d/`
(one for each of IPv4 and IPv6).

All profiles/gateways are SIP/TLS and SRTP-only,
though dialplan extension params must enforce SRTP on outbound legs.

## Inbound

All three inbound profiles accept calls
without authorization
in the `inbound` context,
which simply imports
extensions from `dialplan/inbound.xml.d/`

## Outbound

The outbound profile is not intended
to receive inbound calls,
so it authorizes calls
and accepts calls in
the non-existent `reject` context.

### Gateways

The outbound profile
imports gateways from `gateways/outbound.d/`.

#### VoIP.ms

Copy `gateways/voip.ms-template.xml`
to `gateways/outbound.d/`
and replace template variables.


# Dialplan

The config contains
a single `inbound` dialplan context
which simply imports extensions from `dialplan/inbound.xml.d/`.

## Relay

Copy `diaplan/relay-template.xml`
to `dialplan/inbound.xml.d/`
and replace template values.

To call through the gateway,
call `<dialed_number>@<relay_domain>:<relay_inbound_sip_port>`
(with a SIP/TLS and SRTP-capable client)
and if there's an extension matching `<dialed_number>`,
the call goes through the gateway
to `<destination_number>`.
