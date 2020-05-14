
# kubectl-aliases-powershell

*Forked from: https://github.com/ahmetb/kubectl-aliases*

This repository contains [a script](generate_aliases.py) to generate hundreds of convenient kubectl PowerShell aliases programmatically.

### Examples

Some of the 800 generated aliases are:

```powershell
function k() { & kubectl $args }
function kg() { & kubectl get $args }
function kgpo() { & kubectl get pods $args }

function ksysgpo() { & kubectl --namespace=kube-system get pods $args }

function krm() { & kubectl delete $args }
function krmf() { & kubectl delete --recursive -f $args }
function krming() { & kubectl delete ingress $args }
function krmingl() { & kubectl delete ingress -l $args }
function krmingall() { & kubectl delete ingress --all $args }

function kgsvcoyaml() { & kubectl get service -o=yaml $args }
function kgsvcwn() { & kubectl get service --watch --namespace $args }

function kgwf() { & kubectl get --watch --recursive -f $args }
...
```

See [the full list](kubectl_aliases.ps1).

### Installation
You can directly download the [`kubectl_aliases.ps1`](kubectl_aliases.ps1) file and save it to PowerShell profile directory: `$Home\Documents\WindowsPowerShell\` then run this command to edit your `profile.ps1`:

```powershell
'. $Home\Documents\WindowsPowerShell\kubectl_aliases.ps1' | Out-File $PROFILE.CurrentUserAllHosts -Encoding ascii -Append
```

To print the full command before running it, add this to your `profile.ps1` file:

```powershell
function kubectl() { Write-Output "> kubectl $(@($args | ForEach-Object {$_}) -join ' ')"; & kubectl.exe $args; }
```

### Syntax explanation

* **`k`**=`kubectl`
  * **`sys`**=`--namespace kube-system`
* commands:
  * **`g`**=`get`
  * **`d`**=`describe`
  * **`rm`**=`delete`
  * **`a`**:`apply -f`
  * **`ex`**: `exec -i -t`
  * **`lo`**: `logs -f`
* resources:
  * **`po`**=pod, **`dep`**=`deployment`, **`ing`**=`ingress`,
    **`svc`**=`service`, **`cm`**=`configmap`, **`sec`=`secret`**,
    **`ns`**=`namespace`, **`no`**=`node`
* flags:
  * output format: **`oyaml`**, **`ojson`**, **`owide`**
  * **`all`**: `--all` or `--all-namespaces` depending on the command
  * **`sl`**: `--show-labels`
  * **`w`**=`-w/--watch`
* value flags (should be at the end):
  * **`f`**=`-f/--filename`
  * **`l`**=`-l/--selector`

### FAQ

**Does this not slow down my shell start up?**

Sourcing the file that contains
~800 aliases takes less than 30 milliseconds on my laptop. Measure it yourself with `Measure-Command { . $Home\Documents\WindowsPowerShell\kubectl_aliases.ps1 }`
command.

**How to regenerate the PowerShell aliases file?**
```powershell
# powershell
generate_aliases.py --output ps1 | Out-File -Encoding ascii kubectl_aliases.ps1
```

-----

This is not an official Google project.
