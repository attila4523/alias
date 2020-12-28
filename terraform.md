# Terraform

## Init

```bash
alias ti="rm -rf .terraform/ && terraform init -backend-config=backend.conf"
```

## Workspace

```bash
alias tl="terraform workspace list"

alias ts="terraform workspace select staging"
alias tp="terraform workspace select production"
alias tu="terraform workspace select uat"
alias tt="terraform workspace select training"
alias td="terraform workspace select dev"
```

## Plan

```bash
alias tps="terraform plan --out=staging.tfplan -var-file=staging.tfvars | landscape"
alias tpp="terraform plan --out=production.tfplan -var-file=production.tfvars | landscape"
alias tpu="terraform plan --out=uat.tfplan -var-file=uat.tfvars | landscape"
alias tpt="terraform plan --out=training.tfplan -var-file=training.tfvars | landscape"
alias tpd="terraform plan --out=dev.tfplan -var-file=dev.tfvars | landscape"
alias tpf="terraform plan --out=fea.tfplan -var-file=fea.tfvars | landscape"
alias tpp2="terraform plan --out=pr2.tfplan -var-file=pr2.tfvars | landscape"
alias tppd="terraform plan --out=prd.tfplan -var-file=prd.tfvars | landscape"
```

## Apply

```bash
alias tas="terraform apply staging.tfplan"
alias tap="terraform apply production.tfplan"
alias tau="terraform apply uat.tfplan"
alias tat="terraform apply training.tfplan"
alias tad="terraform apply dev.tfplan"
alias taf="terraform apply fea.tfplan"
alias tap2="terraform apply pr2.tfplan"
alias tapd="terraform apply prd.tfplan"
```

## Custom Plan

### Plan these modules
```bash
function tpati(){
    if [[ ! -z "$1" ]];
    then
        plan="$1";
        shift 1;
    else 
        exit;
    fi

    if [[ ! -z "$1" ]];
    then
        str="$*"
        target="-target=${str}"
    fi
    
    echo "terraform plan --out=${plan}.tfplan -var-file=${plan}.tfvars ${target} | landscape"
    sleep 3
    eval "terraform plan --out=${plan}.tfplan -var-file=${plan}.tfvars ${target}" | landscape
}
```

Example for usage:

```bash
tpati staging module.website

# OR multiple module

tpati staging module.website -target=module.waf
```

### Don't plan these modules
```bash
function tpatino(){
    if [[ ! -z "$1" ]];
    then
        plan="$1";
        shift 1;
    else 
        exit;
    fi

    if [[ ! -z "$1" ]];
    then
        str="$*"
        target="${str}"
    fi
    
    echo "terraform plan --out=${plan}.tfplan -var-file=${plan}.tfvars $(for r in `terraform state list | cut -d'.' -f-2 | sort | uniq | grep -v ${target}` ; do printf "-target ${r} "; done)| landscape"
    sleep 3
    eval "terraform plan --out=${plan}.tfplan -var-file=${plan}.tfvars $(for r in `terraform state list | cut -d'.' -f-2 | sort | uniq | grep -v ${target}` ; do printf "-target ${r} "; done)" | landscape
}
```

Example for usage:

```bash
tpatino staging module.website

# OR

tpatino staging website
```