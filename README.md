# CSC458_A2

## Running the project
This project does not have external dependencies. In order to run this project, please `cd` into the project folder and run the following shell command 'sudo sh ./run.sh'. Then, the desired results should be in the designated subfolder specified in the handout, and a `fetch_statistics.txt` file should also appear which contains the fetching statistics for buffer size 100 and 20.

## Questions
1.  Why do you see a difference in webpage fetch times with small and large router buffers?
    Fetch times:
        Buffer size: 20
            Average: 0.602577777778 
            Standard deviation: 0.14941765807
        Buffer size: 100
            Average: 1.3493030303 
            Standard deviation: 0.454258313345

    The results show that the router with a larger buffer will have a longer fetch time than the router with a smaller buffer. The reason why this happens is that the immediate link connected to h1 has a high bandwidth while the uplink connected to h2 has a low bandwidth. When the router between them has a large buffer size, h1 will keep increasing CWND as it sees no loss and packets will fill up the buffer quickly. Then, if a new packet arrives at the buffer, it will either be dropped (h1 will back off which increases latency) or wait for a long time to be processed. This adds a great amount of latency to the entire process. Therefore, when the buffer is heavily loaded, the larger the router's buffer size, the larger the latency.

2.  Bufferbloat can occur in other places such as your network interface card (NIC). Check the output of ifconfig eth0 on your VirtualBox VM. What is the (maximum) transmit queue length on the network interface reported by ifconfig? For this queue size and a draining rate of 100 Mbps, what is the maximum time a packet might wait in the queue before it leaves the NIC?
    Max transmit queue length: 1000
    MTU size: 1500 bytes
    Draining rate: 100 Mbps

    The Maximum time a packet might wait = `1000 * 1500 * 8 / 10^8` = 0.12s


3.  How does the RTT reported by ping vary with the queue size? Write a symbolic equation to describe the relation between the two (ignore computation overheads in ping that might affect the final result).
    By comparing the RTT graphs, RTT will change linearly with the queue size. Suppose the queue size is Q, `RTT = c * Q` where `c` is a constant factor.

4.  Identify and describe two ways to mitigate the bufferbloat problem.
    1. Increase the bandwidth of the uplink (bottleneck) so that packets in the buffer can be processed and transmitted faster
    2. Utilize a smart queue management system to control the fullness of the queue so that the queue will not overfill