Title: Architecture
Order: 10
Date: 2018-03-29
Slug: architecture
Authors: elois

## Organisation en modules

::uml::

@startuml

title Architecture Generale

package "Libraries" {
    [crypto]
    [conf]
    [documents]
    [module]
    [network]
    [wotb]
    [dal]
}

package "Main" {
    [conf] ---> [load_conf]
    [module] ---> [create_channels]
    [module] ---> [init_modules]
    [start_services]
    [pass_hand_to_common]
}

package "CriticalModules" {
  [start_services] ---> [cli]
  [start_services] ---> [ws2p]
}

package "Common" {
    [pass_hand_to_common] ---> [common]
    [dal] ---> [instance_dal]
    [ws2p] ---> [listen_network_events]
    [listen_dal_requests]
}

package "DefaultModules" {
  [start_services] ---> [prover]
  [start_services] ---> [ui]
  package "Gva" {
        [start_services] ---> [gva]
        [ws2p] ---> [gva_listen_network_events]
        [common] ---> [gva_listen_blockchain_events]
    }
}

package "OptionalModules" {
    package "Dasa" {
        [start_services] ---> [dasa]
        [ws2p] ---> [dasa_listen_network_events]
        [common] ---> [dasa_listen_blockchain_events]
    }
}

@enduml

::end-uml::
