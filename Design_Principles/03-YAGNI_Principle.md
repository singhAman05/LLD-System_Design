## What is the YAGNI Principle?  
  
**YAGNI (You Aren't Gonna Need It)** is a software design principle that encourages developers to build only what is required today and avoid implementing features based on future assumptions.  
  
The core idea is simple:  
  
> Don't build for tomorrow. Build for today's requirements.  
  
YAGNI originated from **Extreme Programming (XP)** and promotes delivering the simplest solution that satisfies current needs.  
  
---  
  
## 🔑 Key Principles  
  
### 1. Build for Today's Needs  
  
Implement only the functionality required by the current specification.  
  
Avoid creating infrastructure for future possibilities that may never occur.  
  
---  
  
### 2. Avoid Speculative Features  
  
Questions like:  
  
- What if we support multiple providers?  
- What if we add plugins later?  
- What if another team needs this?  
  
should not drive today's implementation unless there is a real requirement.  
  
---  
  
### 3. Keep Solutions Simple  
  
The simplest implementation that satisfies current requirements is usually the best solution.  
  
Simple code is:  
  
- Easier to understand  
- Easier to test  
- Easier to debug  
- Easier to modify  
  
---  
  
### 4. Refactor When Necessary  
  
YAGNI does not mean ignoring future changes.  
  
Instead of preparing for every possible future scenario, refactor the code when new requirements actually appear.  
  
---  
  
### 5. Focus on Delivering Value  
  
Every feature should solve a current business problem.  
  
Code that serves no user requirement adds complexity without delivering value.  
  
---  
  
## ⚠️ Common YAGNI Violations  
  
### Premature Abstractions  
  
Creating interfaces before multiple implementations exist.  
  
```cpp  
class IStorageProvider {  
public:  
virtual void store() = 0;  
};  
```  
  
when there is only one storage implementation.  
  
---  
  
### Unused Factory Patterns  
  
Creating factories for a single object type.  
  
```cpp  
class PaymentFactory {  
public:  
static Payment create();  
};  
```  
  
when only one payment implementation exists.  
  
---  
  
### Plugin Systems for One Plugin  
  
Building extension mechanisms before there are actual extensions.  
  
---  
  
### Excessive Configuration  
  
Adding dozens of configurable options that nobody changes.  
  
---  
  
## 🚫 Overengineered Example  
  
### Requirement  
  
Upload a profile picture:  
  
1. Accept image  
2. Resize image  
3. Save image locally  
  
### YAGNI Violation  
  
```text  
ImageUploader  
│  
├── IMediaHandler  
├── ImageHandler  
├── VideoHandler  
├── MediaHandlerFactory  
├── IStorageProvider  
├── LocalStorage  
└── CloudStorageAdapter  
```  
  
Most of these classes solve problems that do not exist.  
  
---  
  
## ✅ YAGNI Applied  
  
```cpp  
class ImageUploader {  
public:  
void upload(Image image) {  
image.resize(300, 300);  
saveToLocalStorage(image);  
}  
};  
```  
  
Benefits:  
  
- Meets current requirements  
- Easy to understand  
- Easy to test  
- Easy to extend later  
  
---  
  
## ❌ Problems Caused by Ignoring YAGNI  
  
### 1. Wasted Development Time  
  
Building unused features consumes engineering effort that could be spent on real requirements.  
  
---  
  
### 2. Increased Complexity  
  
Extra abstractions make systems harder to understand and maintain.  
  
---  
  
### 3. Delayed Delivery  
  
Time spent preparing for hypothetical futures delays shipping actual value.  
  
---  
  
### 4. Higher Maintenance Cost  
  
Unused code still requires:  
  
- Testing  
- Reviews  
- Bug fixes  
- Dependency updates  
  
Dead code is not free.  
  
---  
  
## 🎯 Benefits of Following YAGNI  
  
- Faster development  
- Simpler codebase  
- Lower maintenance cost  
- Easier onboarding  
- Faster delivery of business value  
- Reduced technical debt  
  
---  
  
## ⚖️ When YAGNI Can Be Relaxed  
  
### Security & Compliance Requirements  
  
Requirements such as:  
  
- Audit logging  
- Encryption  
- Access control  
  
must often be implemented from the beginning.  
  
These are known requirements, not speculative ones.  
  
---  
  
### Architectural Constraints  
  
Some systems have known requirements for:  
  
- High availability  
- Disaster recovery  
- Cross-region replication  
- Performance guarantees  
  
These constraints justify early architectural decisions.  
  
---  
  
### Frameworks & Libraries  
  
Reusable libraries may require more upfront design because future consumers depend on stable APIs.  
  
Even then, start small and expand based on actual usage.  
  
---  
  
## 🔗 Related Principles  
  
- [[01-DRY_Principle]]  
- [[02-KISS_Principle]]  
- [[04-SOLID_Principles]]  
  
---  
  
## 📝 Interview Takeaway  
  
When designing a system:  
  
1. Build only what the current requirements demand.  
2. Avoid speculative abstractions.  
3. Prefer simple solutions over flexible ones.  
4. Refactor when new requirements emerge.  
5. Optimize for today's problems, not tomorrow's guesses.  
  
A good engineer does not predict every future requirement.  
  
A good engineer builds clean code that can evolve when future requirements actually arrive.