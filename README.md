# kubernetes-aliases
Some usefull kubectl (and other kubernetes releated) aliases, making it less painfull typing the whole command.

You can either add copy paste these functions and aliases to your '.bashrc' or create a new file e.g. '.kube_aliases' and add this line to your '.bashrc' ```source kube_aliases```. After you save the file, you can type ```source .bashrc``` to your terminal to use them on the spot.

```bash
# Kubernetes aliases and functions
function kdp(){
        pod=$1
        sudo kubectl describe pod $pod -n $(sudo kubectl get pods -A | grep $pod | awk '{print $1}')
}
function kl(){
        pod=$1
        sudo kubectl logs $pod -n $(sudo kubectl get pods -A | grep $pod | awk '{print $1}')
}
function klf(){
        pod=$1
        sudo kubectl logs $pod -n $(sudo kubectl get pods -A | grep $pod | awk '{print $1}') -f
}
function kd(){
        resource=$1
        resource_name=$2
        sudo kubectl describe $resource $resource_name -n $(sudo kubectl get $resource -A | grep $resource_name | awk '{print $1}')
}
function kg(){
        resource=$1
        namespace=$2
        if [ -z "$namespace" ]
        then
                sudo kubectl get $resource
        else
                sudo kubectl get $resource -n $namespace
        fi
}
function kgp(){
        namespace=$1
        if [ -z "$namespace" ]
        then
                sudo kubectl get pods
        else
                sudo kubectl get pods -n $namespace
        fi
}
function kgpo(){
        namespace=$1
        if [ -z "$namespace" ]
        then
                sudo kubectl get pods -o wide
        else
                sudo kubectl get pods -n $namespace -o wide
        fi
}
function wkgp(){
        namespace=$1
        if [ -z "$namespace" ]
        then
                watch sudo kubectl get pods
        else
                watch sudo kubectl get pods -n $namespace
        fi

}
function wkgpo(){
        namespace=$1
        if [ -z "$namespace" ]
        then
                watch sudo kubectl get pods -o wide
        else
                watch sudo kubectl get pods -n $namespace -o wide
        fi

}
function ktn(){
        node_name=$1
        if [ -z $node_name ] ;
        then
                echo "no node provided.. exiting.."
        else
                sudo kubectl taint nodes $node_name key1=value1:NoSchedule
        fi
}
function krtn(){
        node_name=$1
        if [ -z $node_name ] ;
        then
                echo "no node provided.. exiting.."
        else
                sudo kubectl taint nodes $node_name key1-
        fi
}

function ke(){
        pod=$1
        if [ -z $pod ] ;
        then
                echo "no pod provided.. exiting.."
        else
                sudo kubectl exec -it $pod -n $(sudo kubectl get pods -A | grep $pod | awk '{print $1}') -- bash
        fi
}

alias k='sudo kubectl'
alias kubectl='sudo kubectl'

# Helm aliases

alias h3='sudo helm3'
alias h3i='sudo helm3 install'
alias h3d='sudo helm3 delete'

# Docker aliases

alias dps='sudo docker ps'
alias di='sudo docker images'
alias dig='sudo docker images | grep -i '

#kubectl & helm bash completion
source <(kubectl completion bash)
complete -F __start_kubectl k
source <(helm completion bash)

```

# Other Usefull commands

``` bash
ssh -i <pem if needed> -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null <user-name>@<IP>
```
