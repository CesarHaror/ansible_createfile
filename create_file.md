---
- name: Validar y modificar archivo
  hosts: all
  vars:
    archivo_destino: /home/archivo.txt
    contenido_variable: "Este es el contenido que quieres agregar al archivo."

  tasks:
    - name: Verificar si el archivo existe
      stat:
        path: "{{ archivo_destino }}"
      register: archivo_info

    - name: Crear archivo si no existe
      file:
        path: "{{ archivo_destino }}"
        state: touch
      when: not archivo_info.stat.exists

    - name: Agregar contenido al archivo
      lineinfile:
        path: "{{ archivo_destino }}"
        line: "{{ contenido_variable }}"
        create: yes
      when: archivo_info.stat.exists
