# **Rust Tutorial & Exercise**
**Muhammad Hilal Darul Fauzan**<br/>
**2206830542**<br/>
**Pemrograman Lanjut C**<br/>

## **Tutorial Modul 11: Deployment & Monitoring**

### Reflection on Hello Minikube

1. **Compare the application logs before and after you exposed it as a Service. Try to open the app several times while the proxy into the Service is running. What do you see in the logs? Does the number of logs increase each time you open the app?**

    Ya, terdapat perbedaan setelah service di-expose karena service tersebut dapat menerima request. Ketika saya melakukan refresh berkali-kali terhadap service hello-node, log akan mencatat semua request yang diterima. Misalnya, log mungkin akan menunjukkan entri seperti ini setiap kali ada permintaan baru:

    ![alt text](images/image.jpg)

2. **Notice that there are two versions of `kubectl get` invocation during this tutorial section. The first does not have any option, while the latter has `-n` option with value set to `kube-system`. What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created?**

    Namespace dalam Kubernetes adalah mekanisme untuk mengisolasi grup resource tertentu dalam satu cluster. Namespace dibuat ketika pengguna terpisah menjadi beberapa tim atau proyek.

    Ada dua versi command `kubectl get`:
    - Tanpa `-n`: Perintah ini mengambil informasi dari namespace default, yaitu dari namespace tempat saya bekerja dalam cluster Kubernetes.
    - Dengan `-n`: `n` adalah singkatan dari namespace. Perintah ini mengambil informasi dari namespace tertentu saja. Pada tutorial ini, informasi diambil hanya dari namespace `kube-system`.

    Output dari command tidak menyebutkan pods/service yang telah saya buat karena mereka berada dalam namespace `default`, bukan `kube-system`.

### Reflection on Rolling Update & Kubernetes Manifest File

1. **What is the difference between Rolling Update and Recreate deployment strategy?**

    - Rolling Update: Strategi deployment default yang digunakan oleh Kubernetes di mana ketika kita melakukan update pada deployment, pods akan diperbarui secara bertahap dengan menggantikan versi lama dari pod dengan versi yang lebih baru. Hal ini memastikan tidak ada downtime. Selain itu, jika terjadi masalah selama proses update, Kubernetes akan melakukan rollback ke versi lama yang lebih stabil.
    - Recreate: Strategi di mana semua pods yang ada harus dihentikan terlebih dahulu sebelum membuat versi yang lebih baru. Akibatnya, akan ada downtime selama proses update. Strategi ini biasanya digunakan ketika aplikasi kita tidak dapat menjalankan pod dengan versi lama dan baru secara bersamaan.

2. **Try deploying the Spring Petclinic REST using Recreate deployment strategy and document your attempt.**

    Ya, saya berhasil menerapkan Recreate deployment strategy dengan mengikuti tutorial di https://dev.to/cloudskills/kubernetes-deployment-strategy-recreate-3kgn bagian manual recreate deployment. Pertama, saya menjalankan command `kubectl edit deployments spring-petclinic-rest` dan kemudian mengedit versi dari file teks yang muncul. Setelah itu, Kubernetes secara otomatis menerapkan Recreate deployment strategy.

3. **Prepare different manifest files for executing Recreate deployment strategy.**
    
    Saya sudah melampirkan pada file `deploymentrecreate.yaml`.

4. **What do you think are the benefits of using Kubernetes manifest files? Recall your experience in deploying the app manually and compare it to your experience when deploying the same app by applying the manifest files (i.e., invoking `kubectl apply -f` command) to the cluster.**

    - Manifest files: Manifest files menjelaskan status deployment yang diinginkan, termasuk jumlah replika, image container, resource, dan lainnya. Keuntungan menggunakan manifest files adalah lebih cepat karena kita hanya perlu menjalankan `kubectl apply -f`.
    - Kolaborasi dan versioning: Menyimpan manifest files cocok dengan Git karena perubahannya terlihat jelas dan memungkinkan kolaborasi antar tim.