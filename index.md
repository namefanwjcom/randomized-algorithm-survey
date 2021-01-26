<div align='center' ><font size='70'>随机算法调研报告</font></div>

## 什么是随机算法？

- 维基百科中的定义： A randomized algorithm is an algorithm that employs a degree of
  randomness as part of its logic.
- 我的定义：使用了随机数的算法，并且算法的行为是随机的，其中算法的行为包括算
  法输出的解的正确性/质量、终止性、算法消耗的资源 (时间复杂度、空间复杂度) 等。  

# 随机算法的作用/代价

作用：

- 降低时间 (空间) 复杂度
- 容易实现

代价：

- 算法可能产生不正确的解/不终止

- 随机算法的分析比确定性算法更困难  

## 随机算法的技术手段

- Foiling an adversary(Avoiding worst-case inputs)
- Random Sampling
- Abundance of witnesses
- Random Rounding
- Load Balancing
- Symmetry breaking

## 随机算法的应用

- combinatorial problem solving/optimization

- data structure implementation

- parallel computing

- concurrent/distributed algorithms

  随机的作用

  - 解决确定性算法无法解决的问题，代价是算法以很小的概率不正确或不终止，如异步网络模型下的拜占庭一致性问题被证明用确定性算法无法解决，但可以用随机算法来解决，代价是算法有可能不终止(以1的概率终止)
  - 减小时间/空间复杂度，突破确定性算法的复杂度下界

  算法分类

  - problems
    - consensus
    - leader election
    - mutual exclusion
    - resource allocation
    - atomic object: test-and-set, compare-and-swap, ...
    - $\cdots$

  - communication model

    - shared-memory
    - message-passing(network)
      - different topology of network

  - timing model

    - synchronous

    - asynchronous
    - partially-synchronous

  - failure model

    - connection failure
    - process failure
      - crash failure
      - Byzantine failure

  - adversary(scheduler) model: important for randomized algorithms

    - deterministic adversary
      - adaptive adversary
      - oblivious adversary
      - location-oblivious adversary
      - value-oblivious adversary
      - R/W-oblivious adversary
    - nondeterministic adversary
      - probabilistic adversary

  |                                   | shared-memory | synchronous network | asynchronous network |
  | --------------------------------- | ------------- | ------------------- | -------------------- |
  | leader election                   | 1             | 2                   | 3                    |
  | consensus with connection failure |               | 4                   |                      |
  | consensus with crash failure      | 5             |                     | 6                    |
  | consensus with Byzantine failure  |               | 7                   | 8                    |
  | resource allocation               | 9             |                     |                      |
  | mutual exclusion                  | 10            |                     |                      |
  | test-and-set                      | 11            |                     |                      |
  | $\vdots$                          |               |                     |                      |

  1. leader election in shared-memory

     - impossibility result for wait-free deterministic implementation from atomic registers [need citation]

     - a wait-free randomized algorithm for location-oblivious adversary [Giakkoupis 2012]
     
  2. leader election in synchronous  network

     - impossibility result for anonymous synchronous ring [Angluin 1980]

     - a randomized algorithm for anonymous synchronous ring, terminate with probability 1 and bounded expected time [Higham 1988]
     
  3. leader election in asynchronous network

     - a randomized algorithm for asynchronous ring network to reduce average-case time complexity [Chang 1979]

     - impossibility result for anonymous asynchronous ring [Angluin 1980]
     - a  randomized algorithm for anonymous synchronous ring, terminates with probability 1 and bounded expected time [Higham 1988]
     
  4. consensus with connection failure in synchronous network

     - impossibility result for deterministic algorithms [Gray 1978]

     - a randomized algorithm at the cost of a small probability of error [Varghese 1992]
     
  5. consensus with crash failure in shared-memory

     - impossibility result for deterministic algorithms tolerating one crash failure [Loui 1987]
     - a wait-free randomized algorithm for weak adversary, terminates with probability 1 and polynomial expected time [Chor 1994]
     - a wait-free randomized algorithm for strong adversary, terminates with probability 1 and exponential expected time [Abrahamson 1988]
     - a wait-free randomized algorithm for value-oblivious adversary with logarithmic expected time [Aumann 1997]
     
  6. consensus with crash failure in asynchronous network

     - impossibility result for deterministic algorithms tolerating one crash failure [Fischer 1985]
     - an $f$-resilient randomized algorithm for $n>2f$ processes, terminates with probability 1 and exponential expected time [Ben-Or 1983]
     - an $f$-resilient randomized algorithm for $n>2f$ processes, terminates with probability 1 and constant expected time [Canetti 1993]
     
  7. consensus with Byzantine failure in synchronous network

     - deterministic algorithms solving consensus with $f$ Byzantine failures requires at least $f+1$ rounds [Dolev 1983]

     - an $f$-resilient randomized algorithm with constant expected time and always terminates in $f+2$ rounds [Goldreich 1990]

  8. consensus with Byzantine failure in asynchronous network
     - impossibility result for deterministic algorithms tolerating one Byzantine failure [Fischer 1985]
     - an $f$-resilient randomized algorithm for $n>5f$ processes, terminates with probability 1 and exponential expected time [Ben-Or 1983]
     - an $f$-resilient randomized algorithm for $n>3f$ processes, terminates with probability 1 and constant expected time [Canetti 1993]

  9. resource allocation in shared-memory

     - impossibility result for deterministic symmetric algorithm for the Dining Philosophers problem [Lehmann 1981]
     - a randomized symmetric algorithm, with probability 1 is deadlock-free [Lehmann 1981]
     - a randomized symmetric algorithm, with probability 1 is lockout-free [Lehmann 1981]

  10. mutual exclusion in shared-memory

      - deterministic algorithms for $n$-process mutual exclusion with bounded waiting property needs an RMW variable of at least $O(\log\ n)$ bits [Burns 1982]
      - a randomized algorithm guarantees expected bounded waiting using an RMW variable of $O(\log\log\ n)$ bits [Kushilevitz 1992]

  11. test-and-set in shared-memory

      - impossibility result for wait-free deterministic implementation from atomic registers [Herlihy 1994]
      - a wait-free randomized algorithm for  the adaptive adversary with time complexity $O(\log\ n)$, where $n$ is the number of processes [Arek 1992]
      - a wait-free randomized algorithm for  the R/W-oblivious adversary with time complexity $O(\log\log\ n)$, where $n$ is the number of processes [Alistarh 2011]
      - a wait-free randomized algorithm for  the location-oblivious adversary with time complexity $O(\log^*\ k)$, where $k$ is the maximum contention [Giakkoupis 2012]


  我认为比较有趣的问题

  - 不同的算法会假设不同的adversary model，对应不同的语义。程序的行为是由程序和adversary共同决定的。POPL'19[Tassarotti 2019]中提出的并发随机程序的program logic是针对adaptive adversary设计的，只能用来证明这种adversary model下设计的算法。我想能不能提出一个统一的program logic，通过在rule中加入一些参数，或者直接把adversary作为程序的一部分，来验证不同adversary model下的程序性质。
  - 很多随机的并发/分布式算法是在保证safety的前提下以较大的概率满足liveness或者提高效率，这些算法的safety是确定的而liveness和时间复杂度是概率相关的。我目前知道的两个并发随机程序验证的program logic[Tassarotti 2019, McIver 2016]都是partial-correctness logic，不能用来证明liveness。有一些用来证明终止概率[Ferrer 2015, McIver 2017, Huang 2019]和期望时间复杂度[Kaminski 2016, Olmedo 2016]的program logic，但是这些工作不支持并发。

  未来工作计划

  - 调研一下有没有对各种不同的adversary model进行形式化的工作。算法中对adversary是用自然语言来描述的，有些地方的描述感觉并不清晰。
  - 调研一下对某些具体的并发/分布式随机算法进行验证的工作，虽然通用的验证并发/分布式随机程序的program logic比较少，但是也有一些针对某个具体算法进行验证的工作[Pogosyants 2000, McIver 1998, Kwiatkowska 2002, Bertrand 2019]，看看他们用的方法/模型是否有相似之处，能不能generalize。

- cryptography

- machine learning

- differential privacy

- quantum computing

## 参考文献

[Gary 1978] Gray, James N. "Notes on data base operating systems." *Operating Systems*. Springer, Berlin, Heidelberg, 1978. 393-481.

[Varghese 1992] Varghese, George, and Nancy A. Lynch. "A tradeoff between safety and liveness for randomized coordinated attack protocols." *Proceedings of the eleventh annual ACM symposium on Principles of distributed computing*. 1992.

[Loui 1987] Loui, Michael C., and Hosame H. Abu-Amara. "Memory requirements for agreement among unreliable asynchronous processes." *Advances in Computing research* 4.163-183 (1987): 5-3.

[Giakkoupis 2012] Giakkoupis, George, and Philipp Woelfel. "On the time and space complexity of randomized test-and-set." *Proceedings of the 2012 ACM symposium on Principles of Distributed Computing*. 2012.

[Chang 1979] Chang, Ernest, and Rosemary Roberts. "An improved algorithm for  decentralized extrema-finding in circular configurations of processes." *Communications of the ACM* 22.5 (1979): 281-283.

[Angluin 1980] Angluin, Dana. "Local and global properties in networks of processors." *Proceedings of the twelfth annual ACM symposium on Theory of computing*. 1980.

[Higham 1988] Higham, Lisa. *Randomized distributed computing on rings*. Diss. University of British Columbia, 1988.

[Chor 1994] Chor, Benny, Amos Israeli, and Ming Li. "Wait-free consensus using asynchronous hardware." *SIAM Journal on Computing* 23.4 (1994): 701-712.

[Abrahamson 1988] Abrahamson, Karl. "On achieving consensus using a shared memory." *Proceedings of the seventh annual ACM Symposium on Principles of distributed computing*. 1988.

[Fischer 1985] Fischer, Michael J., Nancy A. Lynch, and Michael S. Paterson. "Impossibility of distributed consensus with one faulty process." *Journal of the ACM (JACM)* 32.2 (1985): 374-382.

[Ben-Or 1983] Ben-Or, Michael. "„Another Advantage of Free Choice: Completely Asynchronous Agreement Protocols (Extended Abstract).“." *Proceedings of the 2nd ACM Annual Symposium on Principles of Distributed Computing, Montreal, Quebec*. 1983.

[Canetti 1993] Canetti, Ran, and Tal Rabin. "Fast asynchronous Byzantine agreement with optimal resilience." *Proceedings of the twenty-fifth annual ACM symposium on Theory of computing*. 1993.

[Goldreich 1990] Goldreich, Oded, and Erez Petrank. "The best of both worlds:  Guaranteeing termination in fast randomized byzantine agreement  protocols." *Information Processing Letters* 36.1 (1990): 45-49.

[Dolev 1983] Dolev, Danny, and H. Raymond Strong. "Authenticated algorithms for Byzantine agreement." *SIAM Journal on Computing* 12.4 (1983): 656-666.

[Lehmann 1981] Lehmann, Daniel, and Michael O. Rabin. "On the advantages of free  choice: A symmetric and fully distributed solution to the dining  philosophers problem." *Proceedings of the 8th ACM SIGPLAN-SIGACT symposium on Principles of programming languages*. 1981.

[Kushilevitz 1992] Kushilevitz, Eyal, and Michael O. Rabin. "Randomized mutual exclusion algorithms revisited." *Proceedings of the eleventh annual ACM symposium on Principles of distributed computing*. 1992.

[Burns 1982] Burns, James E., et al. "Data requirements for implementation of n-process mutual exclusion using a single shared variable." *Journal of the ACM (JACM)* 29.1 (1982): 183-205.

[Herlihy 1994] Herlihy, Maurice, and Nir Shavit. "A simple constructive computability theorem for wait-free computation." *Proceedings of the twenty-sixth annual ACM symposium on Theory of computing*. 1994.

[Arek 1992] Afek, Yehuda, et al. "Wait-free test-and-set." *International Workshop on Distributed Algorithms*. Springer, Berlin, Heidelberg, 1992.

[Alistarh 2011] Alistarh, Dan, and James Aspnes. "Sub-logarithmic test-and-set against a weak adversary." *International Symposium on Distributed Computing*. Springer, Berlin, Heidelberg, 2011.

[Aumann 1997] Aumann, Yonatan. "Efficient asynchronous consensus with the weak adversary scheduler." *Proceedings of the sixteenth annual ACM symposium on Principles of distributed computing*. 1997.

[Tassarotti 2019] Tassarotti, Joseph, and Robert Harper. "A separation logic for concurrent randomized programs." *Proceedings of the ACM on Programming Languages* 3.POPL (2019): 1-30.

[McIver 2016] McIver, Annabelle, Tahiry Rabehaja, and Georg Struth. "Probabilistic rely-guarantee calculus." *Theoretical Computer Science* 655 (2016): 120-134.

[Kaminski 2016]Kaminski, Benjamin Lucien, et al. "Weakest precondition reasoning for expected run–times of probabilistic programs." *European Symposium on Programming*. Springer, Berlin, Heidelberg, 2016.

[Olmedo 2016] Olmedo, Federico, et al. "Reasoning about recursive probabilistic programs." *2016 31st Annual ACM/IEEE Symposium on Logic in Computer Science (LICS)*. IEEE, 2016.

[Ferrer 2015] Ferrer Fioriti, Luis María, and Holger Hermanns. "Probabilistic termination: Soundness, completeness, and compositionality." *Proceedings of the 42nd Annual ACM SIGPLAN-SIGACT Symposium on Principles of Programming Languages*. 2015.

[McIver 2017] McIver, Annabelle, et al. "A new proof rule for almost-sure termination." *Proceedings of the ACM on Programming Languages* 2.POPL (2017): 1-28.

[Huang 2019] Huang, Mingzhang, et al. "Modular verification for almost-sure termination of probabilistic programs." *Proceedings of the ACM on Programming Languages* 3.OOPSLA (2019): 1-29.

[Pogosyants 2000] Pogosyants, Anna, Roberto Segala, and Nancy Lynch. "Verification of the  randomized consensus algorithm of Aspnes and Herlihy: a case study." *Distributed Computing* 13.3 (2000): 155-186.

[McIver 1998] McIver, A. K. "Quantitative program logic and efficiency in probabilistic distributed algorithms." *Theoretical Computer Science. See QLE98 at [65]* (1998).

[Kwiatkowska 2002] Kwiatkowska, Marta, and Gethin Norman. "Verifying randomized Byzantine agreement." *FORTE*. Vol. 2529. 2002.

[Bertrand 2019] Bertrand, Nathalie, et al. "Verification of randomized consensus algorithms under round-rigid adversaries." *CONCUR 2019-30th International Conference on Concurrency Theory*. 2019.