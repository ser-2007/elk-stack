# Ansible ELK Playbook
 
Tämä PLAYBOOK on tarkoitettu ELK Stackin version 5.x määrittämiseen etäpalvelimelle.


Ensin sinun on varmistettava, että järjestelmääsi on asennettu Ansible.

## Huomautuksia ja vaatimuksia

  - Pelikirja on rakennettu ja testattu Ubuntu 16.04 VM:illä ELK-versioille 5.x
  - Tarvitset Ansiblen asennettuna ja käynnissä
  - Playbook on tällä hetkellä määritetty määrittämään ELK-pino yhdessä Metricbeatin kanssa palvelimen suorituskyvyn valvontaa varten. Myös Filebeatilla on rooli. Sinun tarvitsee vain lisätä Filebeat-rooli [site.yml]-tiedostoosi.
 
  ## Ohjeet
 
  1. Muokkaa Ansible hosts -tiedostoa ('/etc/ansible/hosts') ja lisää 'elkservers'-merkintä palvelimelle, johon haluat asentaa ELK:n. Voit tietysti nimetä isännän haluamallasi tavalla, tämä on vain esimerkki.
  2. Tarkista yhteys ELK-palvelimeen.
  3. Kloonaa tämä repo Ansiblen isännöivän koneen terminaalissa.
  4. CD-levy hakemistoon ja suorita:
  "ansible-playbook site.yml".
 
  PLAYBOOK toistot suoritetaan kohdepalvelimella, joka asentaa ELK:n ja määritetyt beats-lähettäjät.



  ---
#
# Playbook to install the ELK stack + Beats
#
- hosts: elkservers
  remote_user: ubuntu
  become: yes
  become_user: root
  roles:
  - { role: java }
  - { role: elasticsearch }
  - { role: kibana }
  - { role: metricbeat }

