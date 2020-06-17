## **Implementar sitio web en Cloud Run**

Cuando nuestra aplicacion es grande tal vez la sobre carga no sea un problema, ya que debemos preocupar por administrar maquinas virtuales, clústeres,pods.etc, seria una buena idea para las aplicaciones grandes, pero si solo estamos trabajando en hacer visible un sitio web **Cloud Run** es la solución. 



Cloud Run lleva el desarrollo "sin servidor" a los contenedores y puede ejecutarse en sus propios clústeres de Google Kubernetes Engine (GKE) o en una solución PaaS totalmente administrada proporcionada por Cloud Run.



### Diagrama de arquitectura

A continuación puede ver el flujo de la implementación y el alojamiento de Cloud Run.

Comience con una imagen de Docker creada a través de Cloud Build, que se activa desde Cloud Shell, luego implemente la imagen en Cloud Run desde un comando en Cloud Shell.



![](https://res.cloudinary.com/xaiop/image/upload/c_scale,w_567/v1592357674/Ry1hidHMw9wjyWNTvENYln0NxFFyBJQyt1bPC_2Fdp0Qc_3D_ulhjbv.png)