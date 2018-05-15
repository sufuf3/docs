# Summary

* [Installation Guide](README.md)
    * [Bill Of Materials](prereqs/hardware.md)
    * [Networking Connectivity](prereqs/networking.md)
    * Software Requirements
        * Kubernetes
            * [Single Node KB8s](prereqs/minikube.md)
            * [Multi Node KB8s](prereqs/kubespray.md)
        * [Helm](prereqs/helm.md)
        * [Docker Registry](prereqs/docker-registry.md)
        * [OpenStack Integration](prereqs/openstack-helm.md)
    * [Profiles](profiles/intro.md)
        * [RCORD Lite](profiles/rcord-lite.md)
            * [OLT Setup](profiles/olt-setup.md)
        * [MCORD](profiles/mcord.md)
            * [EnodeB Setup](profiles/enodeb-setup.md)
    * [Helm Reference](charts/helm.md)
        * [XOS-CORE](charts/xos-core.md)
        * [ONOS](charts/onos.md)
        * [VOLTHA](charts/voltha.md)
        * [kafka](charts/kafka.md)
    * [Fabric setup](prereqs/fabric-setup.md)
* Operating CORD
    * [Diagnostics](operating_cord/diag.md)
* [Defining Models in CORD](xos/README.md)
    * [XOS Support for Models](xos/dev/xproto.md)
    * [Core Models](xos/core_models.md)
    * [Security Policies](xos/security_policies.md)
    * [Writing Synchronizers](xos/dev/synchronizers.md)
        * [Design Guidelines](xos/dev/sync_arch.md)
        * [Implementation Details](xos/dev/sync_impl.md)
* Developing for CORD
    * [Getting the Source Code](developer/getting_the_code.md)
    * [Developer Workflows](developer/workflows.md)
    * [VTN and Service Composition](xos/xos_vtn.md)
    * [GUI Development](xos-gui/developer/README.md)
        * [Quickstart](xos-gui/developer/quickstart.md)
        * [Service Graph](xos-gui/developer/service_graph.md)
        * [GUI Extensions](xos-gui/developer/gui_extensions.md)
        * [GUI Internals](xos-gui/architecture/README.md)
            * [Module Strucure](xos-gui/architecture/gui-modules.md)
            * [Data Sources](xos-gui/architecture/data-sources.md)
        * [Tests](xos-gui/developer/tests.md)
    * [Unit Tests](xos/dev/unittest.md)
* [Testing CORD](cord-tester/README.md)
    * [Test Setup](cord-tester/qa_testsetup.md)
    * [Test Environment](cord-tester/qa_testenv.md)
    * [System Tests](cord-tester/validate_pods.md)
