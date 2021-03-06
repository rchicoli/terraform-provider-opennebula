# terraform-provider-opennebula

[OpenNebula](https://opennebula.org/) provider for [Terraform](https://www.terraform.io/).
 
* Leverages [OpenNebula's XML/RPC API](https://docs.opennebula.org/5.2/integration/system_interfaces/api.html) 
* Tested for versions 5.X


The provider tries to impose a lightweight level of abstraction on OpenNebula's resources. This means that only the most fundamental attributes are directly accessible (i.e. names, IDs, permissions and user/group identities). For maximum flexibility and portability, the remaining attributes can be specified using any of the formats natively accepted by OpenNebula (XML and String).



## EXAMPLE

Create a file called `demo_template.txt`.

_Notice how we need to escape native variables `$$USER[SSH_PUBLIC_KEY]` with two dollar signs, as terraform will try to replace all variables with a single dollar sign)_

```
CUSTOM_ATTRIBUTE = "$CUSTOM_ATTRIBUTE_VALUE"
CONTEXT = [
  DNS_HOSTNAME = "yes",
  NETWORK = "YES",
  SSH_PUBLIC_KEY = "$$USER[SSH_PUBLIC_KEY]",
  USERNAME = "root" ]
CPU = "0.5"
VCPU = "4"
MEMORY = "3000"
GRAPHICS = [
  KEYMAP = "en",
  LISTEN = "0.0.0.0",
  TYPE = "VNC" ]
```

And the following `terraform.tf` file:

```
provider "opennebula" {
  endpoint = "api's endpoint"
  username = "user's name"
  password = "user's password"
}

data "template_file" "demo" {
  template = "${file("demo_template.txt")}"
  vars = {
    CUSTOM_ATTRIBUTE_VALUE = "demo-me"
  }
}

resource "opennebula_template" "demo" {
  name = "terraform-demo"
  description = "${data.template_file.demo.rendered}"
  permissions = "600"
}

output "demo_template_id" {
  value = "${opennebula_template.demo.id}"
}

output "demo_template_uname" {
  value = "${opennebula_template.demo.uname}"
}
```


## ROADMAP

The following list represent's all of OpenNebula's resources reachable through their API. The checked items are the ones that are fully functional and tested:

* [ ] [onevm](https://docs.opennebula.org/5.2/integration/system_interfaces/api.html#onevm)
* [x] [onetemplate](https://docs.opennebula.org/5.2/integration/system_interfaces/api.html#onetemplate)
* [ ] [onehost](https://docs.opennebula.org/5.2/integration/system_interfaces/api.html#onehost)
* [ ] [onecluster](https://docs.opennebula.org/5.2/integration/system_interfaces/api.html#onecluster)
* [ ] [onegroup](https://docs.opennebula.org/5.2/integration/system_interfaces/api.html#onegroup)
* [ ] [onevdc](https://docs.opennebula.org/5.2/integration/system_interfaces/api.html#onevdc)
* [x] [onevnet](https://docs.opennebula.org/5.2/integration/system_interfaces/api.html#onevnet)
* [ ] [oneuser](https://docs.opennebula.org/5.2/integration/system_interfaces/api.html#oneuser)
* [ ] [onedatastore](https://docs.opennebula.org/5.2/integration/system_interfaces/api.html#onedatastore)
* [ ] [oneimage](https://docs.opennebula.org/5.2/integration/system_interfaces/api.html#oneimage)
* [ ] [onemarket](https://docs.opennebula.org/5.2/integration/system_interfaces/api.html#onemarket)
* [ ] [onemarketapp](https://docs.opennebula.org/5.2/integration/system_interfaces/api.html#onemarketapp)
* [ ] [onevrouter](https://docs.opennebula.org/5.2/integration/system_interfaces/api.html#onevrouter)
* [ ] [onezone](https://docs.opennebula.org/5.2/integration/system_interfaces/api.html#onezone)
* [ ] [onesecgroup](https://docs.opennebula.org/5.2/integration/system_interfaces/api.html#onesecgroup)
* [ ] [oneacl](https://docs.opennebula.org/5.2/integration/system_interfaces/api.html#oneacl)
* [ ] [oneacct](https://docs.opennebula.org/5.2/integration/system_interfaces/api.html#oneacct)


# CONTRIBUTIONS

Contributions are of course open and welcome.
