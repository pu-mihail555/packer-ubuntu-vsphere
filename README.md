# packer-ubuntu-vsphere

*[Русская версия ниже](#русская-версия)*

This repository provides a Packer configuration to automatically build an Ubuntu Server 22.0*/24.0* template in VMware vSphere. There is no need to manually download any ISO files — the process is fully automated.

## Supported Images
* Ubuntu Server 22.0*
* Ubuntu Server 24.0*

## Prerequisites
Ensure you have Packer installed (tested with version `1.10.3`) along with the required plugins. 

Install the plugins using the following commands:
```bash
packer plugins install [github.com/hashicorp/vsphere](https://github.com/hashicorp/vsphere)
packer plugins install [github.com/hashicorp/ansible](https://github.com/hashicorp/ansible)
```

## Usage

### Step 1: Configuration
First, update the `hosts.ini` file to specify the necessary target hosts for Ansible.

Next, export the required environment variables for vCenter authentication and deployment:
```bash
export VCENTER_USERNAME=""
export VCENTER_PASSWORD=""
export VCENTER_SERVER=""
export VCENTER_DATACENTER=""
export VCENTER_CLUSTER=""
export VCENTER_DATASTORE=""
export VCENTER_NETWORK=""
export VCENTER_FOLDER=""
export ISO_PATH=""
```

### Step 2: Certificates Configuration (Optional)
The project supports automated installation of custom certificates during the build process via the `certs/` directory:
* **Corporate Root Certificates:** You can install your own corporate root certificate. Replace the dummy `.crt` file in the `certs/` directory with your actual certificate, and it will be automatically added to the trusted root store on the VM.
* **SSH CA Certificates:** You can also configure SSH certificate-based authentication. Place your SSH CA certificate (the `.pem` file) in the `certs/` directory. If you do not use SSH certificates in your infrastructure, you can safely remove this file and its related references from the Ansible playbook.

### Step 3: Build the Image
Navigate to the root directory of the repository and run the build command:
```bash
packer build -force -on-error=ask -var-file distr_vars/2204.hcl builder.pkr.hcl 
```

**Command Flags Explanation:**
* `-force`: Forces the overwrite of existing virtual machine templates with the same name.
* `-on-error=ask`: Pauses the build if an error occurs and provides a prompt to troubleshoot: `[c] Clean up and exit, [a] abort without cleanup, or [r] retry step`.
* `-var-file`: Specifies the variables file to be used during the build process (e.g., `distr_vars/2204.hcl`).

---

## Русская версия

Этот репозиторий содержит конфигурацию Packer для автоматического создания шаблона виртуальной машины Ubuntu Server 22.0*/24.0* в VMware vSphere. Скачивать ISO-образы вручную не требуется — процесс полностью автоматизирован.

## Поддерживаемые образы
* Ubuntu Server 22.0*
* Ubuntu Server 24.0*

## Требования
Для работы на хост-машине/ранере должен быть установлен Packer (проверена версия `1.10.3`), а также необходимые плагины.

Установите плагины следующими командами:
```bash
packer plugins install [github.com/hashicorp/vsphere](https://github.com/hashicorp/vsphere)
packer plugins install [github.com/hashicorp/ansible](https://github.com/hashicorp/ansible)
```

## Использование

### Шаг 1: Настройка окружения
Отредактируйте файл `hosts.ini`, чтобы выставить необходимые целевые хосты для Ansible.

Затем экспортируйте переменные окружения для подключения к vCenter:
```bash
export VCENTER_USERNAME=""
export VCENTER_PASSWORD=""
export VCENTER_SERVER=""
export VCENTER_DATACENTER=""
export VCENTER_CLUSTER=""
export VCENTER_DATASTORE=""
export VCENTER_NETWORK=""
export VCENTER_FOLDER=""
export ISO_PATH=""
```

### Шаг 2: Настройка сертификатов (опционально)
Проект поддерживает автоматическое добавление ваших сертификатов при сборке образа через папку `certs/`:
* **Корпоративные корневые сертификаты:** Поддерживается установка собственных корневых сертификатов. Замените файл-заглушку `.crt` в папке `certs/` на ваш реальный сертификат, и он будет автоматически добавлен в доверенные на виртуальной машине.
* **SSH CA сертификаты:** Для безопасного подключения к хостам вы можете использовать SSH-сертификаты. Добавьте ваш SSH CA сертификат (файл `.pem`) в папку `certs/`. Если в вашей инфраструктуре не используются SSH-сертификаты, вы можете просто удалить этот файл и убрать соответствующие задачи из плейбука Ansible.

### Шаг 3: Сборка образа
Перейдите в корень директории и выполните команду сборки:
```bash
packer build -force -on-error=ask -var-file distr_vars/2204.hcl builder.pkr.hcl 
```

**Описание используемых флагов:**
* `-force` — используется для перезаписи уже существующих шаблонов виртуальных машин с таким же именем.
* `-on-error=ask` — при возникновении ошибки сборка ставится на паузу и пользователю предлагается выбор дальнейших действий: `[c] Clean up and exit, [a] abort without cleanup, or [r] retry step`.
* `-var-file` — используется для указания файла переменных, который будет применен во время сборки образа (в данном случае из папки `distr_vars`).