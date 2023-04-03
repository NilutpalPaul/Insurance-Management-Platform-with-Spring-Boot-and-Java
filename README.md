# Insurance-Management-Platform-with-Spring-Boot-and-Java



Create a new Spring Boot project in your favorite IDE.

Add the following dependencies to your pom.xml file:

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>


Create a new Java class called InsurancePolicy:

@Entity
public class InsurancePolicy {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    @Column
    private String policyNumber;

    @Column
    private LocalDate startDate;

    @Column
    private LocalDate endDate;

    @Column
    private BigDecimal premiumAmount;

    // Constructor, getters, and setters
}


Create a new Java interface called InsurancePolicyRepository:

@Repository
public interface InsurancePolicyRepository extends JpaRepository<InsurancePolicy, Long> {
}


Create a new Java class called InsurancePolicyService:

@Service
public class InsurancePolicyService {
    @Autowired
    private InsurancePolicyRepository policyRepository;

    public List<InsurancePolicy> getAllPolicies() {
        return policyRepository.findAll();
    }

    public Optional<InsurancePolicy> getPolicyById(Long id) {
        return policyRepository.findById(id);
    }

    public InsurancePolicy savePolicy(InsurancePolicy policy) {
        return policyRepository.save(policy);
    }

    public void deletePolicy(Long id) {
        policyRepository.deleteById(id);
    }
}


Create a new Java class called InsurancePolicyController:

@RestController
@RequestMapping("/policies")
public class InsurancePolicyController {
    @Autowired
    private InsurancePolicyService policyService;

    @GetMapping("")
    public List<InsurancePolicy> getAllPolicies() {
        return policyService.getAllPolicies();
    }

    @GetMapping("/{id}")
    public Optional<InsurancePolicy> getPolicyById(@PathVariable Long id) {
        return policyService.getPolicyById(id);
    }

    @PostMapping("")
    public InsurancePolicy createPolicy(@RequestBody InsurancePolicy policy) {
        return policyService.savePolicy(policy);
    }

    @PutMapping("/{id}")
    public InsurancePolicy updatePolicy(@PathVariable Long id, @RequestBody InsurancePolicy policy) {
        Optional<InsurancePolicy> existingPolicy = policyService.getPolicyById(id);
        if (existingPolicy.isPresent()) {
            InsurancePolicy updatedPolicy = existingPolicy.get();
            updatedPolicy.setPolicyNumber(policy.getPolicyNumber());
            updatedPolicy.setStartDate(policy.getStartDate());
            updatedPolicy.setEndDate(policy.getEndDate());
            updatedPolicy.setPremiumAmount(policy.getPremiumAmount());
            return policyService.savePolicy(updatedPolicy);
        } else {
            return policyService.savePolicy(policy);
        }
    }

    @DeleteMapping("/{id}")
    public void deletePolicy(@PathVariable Long id) {
        policyService.deletePolicy(id);
    }
}


Run the Spring Boot application and test the API endpoints using a tool like Postman.
