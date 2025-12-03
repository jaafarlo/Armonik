# Armonik

ubuntu@ekolasinac-vm:~/ArmoniK$ git fetch 
remote: Enumerating objects: 31, done.
remote: Counting objects: 100% (31/31), done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 31 (delta 20), reused 28 (delta 20), pack-reused 0 (from 0)
Unpacking objects: 100% (31/31), 4.94 KiB | 843.00 KiB/s, done.
From https://github.com/aneoconsulting/ArmoniK
 + 2bd348e4...bfff5496 jf/formation25                    -> origin/jf/formation25  (forced update)
 + ca6b4cc2...1eb03da0 renovate/major-github-actions     -> origin/renovate/major-github-actions  (forced update)
 + a3cc74f2...95a12e9c renovate/major-terraform-provider -> origin/renovate/major-terraform-provider  (forced update)
 + 48f3aa25...cfa0607e renovate/sphinx-9.x 

ubuntu@ekolasinac-vm:~/ArmoniK$ git checkout jf/formation25
branch 'jf/formation25' set up to track 'origin/jf/formation25'.
Switched to a new branch 'jf/formation25'
ubuntu@ekolasinac-vm:~/ArmoniK$ ls
CHANGELOG.md  README.md     cicd               infrastructure     renovate.json  versions.tfvars.json
LICENSE       benchmarking  extra.tfvars.json  jupyter-notebooks  tools


ubuntu@ekolasinac-vm:~/ArmoniK/infrastructure/quick-deploy/localhost$ pwd
# cd /home/ubuntu/ArmoniK/infrastructure/quick-deploy/localhost
#make (il va executer le Make file)

########sur le terminal ##########
ubuntu@ekolasinac-vm:~$ exit
logout
Connection to 34.147.59.54 closed.
ak-formation@AK-formation-4:~$ cat .ssh/config 
Host TP-ArmoniK
  Hostname 34.147.59.54
  IdentityFile ~/.ssh/id_rsa
  User ubuntu
#######################################
acceder a la GUI : 10.100.1.10:5000 >> 34.147.59.54
armonik = {
  "admin_app_url" = "http://10.100.1.10:5000/admin"
  "chaos_mesh_url" = null
  "control_plane_url" = "http://10.100.1.10:5001"
  "grafana_url" = "http://10.100.1.10:5000/grafana/"
  "seq_web_url" = "http://10.100.1.10:5000/seq/"
}

ubuntu@ekolasinac-vm:~/ArmoniK/infrastructure/quick-deploy/localhost$ export AKCONFIG=/home/ubuntu/ArmoniK/infrastructure/quick-deploy/localhost/generated/armonik-cli.yaml
pour lancer une tache :
le lien du worker : https://github.com/aneoconsulting/PymoniK/tree/main
ubuntu@ekolasinac-vm:~$ ls
ArmoniK  PymoniK  kubectl
ubuntu@ekolasinac-vm:~$ cd PymoniK/
ubuntu@ekolasinac-vm:~/PymoniK$ ls
README.md  automation.py  docs  examples  mkdocs.yml  pymonik  pymonik_worker  test_client
ubuntu@ekolasinac-vm:~/PymoniK$ cd test_client/
ubuntu@ekolasinac-vm:~/PymoniK/test_client$ ls
README.md                    estimate_pi.py   materialize_test.py  retrieve_object_test.py  task_options.py       uv.lock
adaptive_vector_addition.py  lambda_tasks.py  pyproject.toml       subtasking.py            uploading_objects.py  worker_cache.py
ubuntu@ekolasinac-vm:~/PymoniK/test_client$ uv run estimate_pi.py 


ak-formation@AK-formation-4:~/ArmoniK/tools$ ls
certs  ci  git  installation  k8s  metrics  mongodb
ak-formation@AK-formation-4:~/ArmoniK/tools
ak-formation@AK-formation-4:~/ArmoniK/tools/mongodb$ ./access-mongo-from-kubernetes-as-user.sh


ubuntu@ekolasinac-vm:~$ cd ArmoniK/tools/m
metrics/ mongodb/ 
ubuntu@ekolasinac-vm:~$ cd ArmoniK/tools/mongodb/
ubuntu@ekolasinac-vm:~/ArmoniK/tools/mongodb$ ./access-mongo-from-kubernetes-as-user.sh 


Installer la cli :
ubuntu@ekolasinac-vm:~$ ls
ArmoniK  PymoniK  kubectl
ubuntu@ekolasinac-vm:~$ pipx install armonik-cli
⢿ installing armonik-cli


Lancer des taches avec les parametres qu'on veut :
ubuntu@ekolasinac-vm:~$ echo $AKCONFIG
/home/ubuntu/ArmoniK/infrastructure/quick-deploy/localhost/generated/armonik-cli.yaml
ubuntu@ekolasinac-vm:~$ 
ubuntu@ekolasinac-vm:~$ cat $AKCONFIG
# ArmoniK Connection Configuration
---
"endpoint": "http://10.100.1.10:5001"
ubuntu@ekolasinac-vm:~$ ^C
ubuntu@ekolasinac-vm:~$ armonik cluster info
╭───────────────────────────────────────────────────────────── Cluster Information ──────────────────────────────────────────────────────────────╮
│ Endpoint:     http://10.100.1.10:5001                                                                                                          │
│ Core Version: 0.34.4                                                                                                                           │
│ API Version:  3.27.0  

###à executer sur le terminal #########
docker run --rm \
    -e HtcMock__NTasks=10000 \
    -e HtcMock__TotalCalculationTime=00:00:10 \
    -e HtcMock__DataSize=1 \
    -e HtcMock__MemorySize=1 \
    -e HtcMock__SubTasksLevels=10\
    -e HtcMock__Partition=htcmock \
    -e HtcMock__EnableFastCompute=true \
    -e HtcMock__TaskRpcException="" \
    -e GrpcClient__Endpoint=http://10.100.1.10:5001 \
    dockerhubaneo/armonik_core_htcmock_test_client:0.34.4
###############################################################

kubectl -n kube kub-system get pods 

Ajouter une partition :
dans le fichier : parameters.tfvars

  # Partition for the Helloworld worker
  helloworld = {
    # number of replicas for each deployment of compute plane
    replicas = 0
    # Socket type used by agent and worker to communicate
    socket_type = "tcp"
    # ArmoniK polling agent
    polling_agent = {
      limits = {
        cpu    = "2000m"
        memory = "2048Mi"
      }
      requests = {
        cpu    = "50m"
        memory = "50Mi"
      }
    }
    # ArmoniK workers
    worker = [
      {
        image = "dockerhubaneo/armonik_demo_helloworld_worker"
        tag= "v2.21.0-SNAPSHOT.78.sha.80a7a9d"
        limits = {
          cpu    = "1000m"
          memory = "1024Mi"
        }
        requests = {
          cpu    = "50m"
          memory = "50Mi"
        }
      }
    ]
    hpa = {
      type              = "prometheus"
      polling_interval  = 15
      cooldown_period   = 300
      min_replica_count = 0
      max_replica_count = 5
      behavior = {
        restore_to_original_replica_count = true
        stabilization_window_seconds      = 300
        type                              = "Percent"
        value                             = 100
        period_seconds                    = 15
      }
      triggers = [
        {
          type      = "prometheus"
          threshold = 2
        },
      ]
    }
  },

apres on va dans : ubuntu@ekolasinac-vm:~/ArmoniK/infrastructure/quick-deploy/localhost$ 
executer : make dans le terminal 
