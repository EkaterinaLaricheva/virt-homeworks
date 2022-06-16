# Домашнее задание к занятию "7.3. Основы и принцип работы Терраформ"

## Задача 1. Создадим бэкэнд в S3 (необязательно, но крайне желательно).

Если в рамках предыдущего задания у вас уже есть аккаунт AWS, то давайте продолжим знакомство со взаимодействием
терраформа и aws. 

1. Создайте s3 бакет, iam роль и пользователя от которого будет работать терраформ. Можно создать отдельного пользователя,
а можно использовать созданного в рамках предыдущего задания, просто добавьте ему необходимы права, как описано 
[здесь](https://www.terraform.io/docs/backends/types/s3.html).
1. Зарегистрируйте бэкэнд в терраформ проекте как описано по ссылке выше. 

РЕШЕНИЕ НЕВОЗМОЖНО

## Задача 2. Инициализируем проект и создаем воркспейсы. 

1. Выполните `terraform init`:
    * если был создан бэкэнд в S3, то терраформ создат файл стейтов в S3 и запись в таблице 
dynamodb.
    * иначе будет создан локальный файл со стейтами. 
    
    ![image](https://user-images.githubusercontent.com/91233405/174126981-d438c071-3f44-4558-8676-09bc8436f6f8.png)

1. Создайте два воркспейса `stage` и `prod`.

![image](https://user-images.githubusercontent.com/91233405/174127053-1bcc02fe-9be6-4523-8565-8b0d6f159f80.png)

3. В уже созданный `aws_instance` добавьте зависимость типа инстанса от вокспейса, что бы в разных ворскспейсах 
использовались разные `instance_type`.

1. Добавим `count`. Для `stage` должен создаться один экземпляр `ec2`, а для `prod` два. 
1. Создайте рядом еще один `aws_instance`, но теперь определите их количество при помощи `for_each`, а не `count`.
1. Что бы при изменении типа инстанса не возникло ситуации, когда не будет ни одного инстанса добавьте параметр
жизненного цикла `create_before_destroy = true` в один из рессурсов `aws_instance`.
1. При желании поэкспериментируйте с другими параметрами и рессурсами.

В виде результата работы пришлите:
* Вывод команды `terraform workspace list`.

![image](https://user-images.githubusercontent.com/91233405/174127324-34cb128d-c61e-4e9b-99d9-02e203917ec7.png)

* Вывод команды `terraform plan` для воркспейса `prod`.  
Ошибка учетных данных. Файл main.tf

vagrant@vagrant:~/terraform$ cat main.tf
provider "aws" {
                region = "us-west-2"

}
resource "aws_instance" "web" {
ami = data.aws_ami.amazon_linux.id
 instance_type = "t3.micro"
tags = {"project": "main"}
lifecycle {
 create_before_destroy = true
 prevent_destroy = true
 ignore_changes = [tags]
 }
 }


data "aws_ami" "amazon_linux" {
most_recent = true
owners = ["amazon"]
filter {
 name = "name"
 values = ["amzn-ami-hvm-*-x86_64-gp2"]
 }
filter {
 name = "owner-alias"
 values = ["amazon"]
 }
}

locals {
web_instance_type_map = {
 stage = "t3.micro"
 prod = "t3.large"
}
}

locals {
web_instance_count_map = {
 stage = 1
 prod = 2
}
}

locals {
instances = {
 "t3.micro" = data.aws_ami.amazon_linux.id
 "t3.large" = data.aws_ami.amazon_linux.id
}
}
