# Instructions
* Install Virtualbox
* Install Virtualbox Guest Additions 
* Install Vagrant
* Clone the repository
* Edit the NODE_COUNT variable in the Vagrantfile to spin up N number of Windows guest vms
*       ```
        vagrant up
        ```

After the box downloads and the N guests spin up, you should be able to ping back and forth. The box was built with WinRM configured so provisioning via powershell is possible.
#### Be aware that Windows firewall is enabled.

Thanks to <https://github.com/ferventcoder> for the base box.
For other base boxes that he has created, check out <https://vagrantcloud.com/ferventcoder>
