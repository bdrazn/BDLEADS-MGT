# 🤝 Contributing to BD Lead Manager

Thank you for your interest in contributing to BD Lead Manager! This document provides guidelines and information for contributors.

## 📋 Table of Contents

- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [Coding Standards](#coding-standards)
- [Architecture Guidelines](#architecture-guidelines)
- [Testing Guidelines](#testing-guidelines)
- [Pull Request Process](#pull-request-process)
- [Code Review Guidelines](#code-review-guidelines)
- [Issue Reporting](#issue-reporting)

## 🚀 Getting Started

### **Prerequisites**
- Xcode 15.0+
- iOS 15.0+ deployment target
- Apple Developer Account
- Firebase project access
- Git and GitHub account

### **Fork and Clone**
1. Fork the repository on GitHub
2. Clone your fork locally:
   ```bash
   git clone https://github.com/your-username/bulk3-copy-12.git
   cd bulk3-copy-12
   ```
3. Add the original repository as upstream:
   ```bash
   git remote add upstream https://github.com/original-owner/bulk3-copy-12.git
   ```

## 🛠️ Development Setup

### **Environment Configuration**
1. Follow the [Installation Guide](INSTALLATION.md)
2. Configure Firebase project
3. Set up StoreKit configuration
4. Verify all dependencies are resolved

### **Development Workflow**
1. Create a feature branch:
   ```bash
   git checkout -b feature/your-feature-name
   ```
2. Make your changes
3. Test thoroughly
4. Commit with descriptive messages
5. Push to your fork
6. Create a pull request

## 📝 Coding Standards

### **Swift Style Guide**

#### **Naming Conventions**
```swift
// ✅ Good
class UserProfileManager {
    var currentUser: User?
    func loadUserData() async { }
}

// ❌ Bad
class userProfileManager {
    var CurrentUser: User?
    func LoadUserData() async { }
}
```

#### **File Organization**
```
Views/
├── Authentication/
│   ├── LoginView.swift
│   ├── RegisterView.swift
│   └── ForgotPasswordView.swift
├── Campaign/
│   ├── CampaignsView.swift
│   └── CreateCampaignForm.swift
└── Components/
    ├── LoadingView.swift
    └── ErrorView.swift
```

#### **Code Structure**
```swift
// MARK: - Imports
import SwiftUI
import Firebase

// MARK: - Model
struct UserProfile: Codable {
    let id: String
    let name: String
    let email: String
}

// MARK: - View Model
@MainActor
class UserProfileViewModel: ObservableObject {
    @Published var profile: UserProfile?
    @Published var isLoading = false
    
    func loadProfile() async {
        // Implementation
    }
}

// MARK: - View
struct UserProfileView: View {
    @StateObject private var viewModel = UserProfileViewModel()
    
    var body: some View {
        // View implementation
    }
}
```

### **Documentation Standards**

#### **Code Comments**
```swift
/// Loads user profile data from Firebase
/// - Parameter userId: The unique identifier of the user
/// - Returns: The user profile data
func loadUserProfile(userId: String) async throws -> UserProfile {
    // Implementation
}
```

#### **Documentation Files**
- Create `.md` files for new features
- Update existing documentation
- Include code examples
- Document breaking changes

## 🏗️ Architecture Guidelines

### **MVVM Pattern**
```swift
// ✅ Good: Clean separation
struct CampaignView: View {
    @StateObject private var viewModel = CampaignViewModel()
    
    var body: some View {
        // View logic only
    }
}

@MainActor
class CampaignViewModel: ObservableObject {
    @Published var campaigns: [Campaign] = []
    
    func loadCampaigns() async {
        // Business logic
    }
}
```

### **Dependency Injection**
```swift
// ✅ Good: Injectable dependencies
class DataManager {
    let firebaseService: FirebaseService
    let localStorage: LocalStorage
    
    init(firebaseService: FirebaseService, localStorage: LocalStorage) {
        self.firebaseService = firebaseService
        self.localStorage = localStorage
    }
}
```

### **Error Handling**
```swift
// ✅ Good: Comprehensive error handling
enum AppError: LocalizedError {
    case networkError(String)
    case authenticationError(String)
    case dataError(String)
    
    var errorDescription: String? {
        switch self {
        case .networkError(let message):
            return "Network error: \(message)"
        case .authenticationError(let message):
            return "Authentication error: \(message)"
        case .dataError(let message):
            return "Data error: \(message)"
        }
    }
}
```

### **User Isolation**
```swift
// ✅ Good: Always implement user isolation
protocol UserOwnedModel {
    var userId: String { get }
}

@Model
final class ContactModel: UserOwnedModel {
    var userId: String
    var name: String
    // ... other properties
}
```

## 🧪 Testing Guidelines

### **Unit Testing**
```swift
import XCTest
@testable import bulk3

class UserProfileViewModelTests: XCTestCase {
    var viewModel: UserProfileViewModel!
    
    override func setUp() {
        super.setUp()
        viewModel = UserProfileViewModel()
    }
    
    func testLoadProfileSuccess() async throws {
        // Given
        let expectedProfile = UserProfile(id: "1", name: "Test", email: "test@example.com")
        
        // When
        await viewModel.loadProfile()
        
        // Then
        XCTAssertEqual(viewModel.profile?.id, expectedProfile.id)
    }
}
```

### **UI Testing**
```swift
import XCTest

class CampaignUITests: XCTestCase {
    func testCreateCampaignFlow() {
        let app = XCUIApplication()
        app.launch()
        
        // Navigate to campaigns
        app.tabBars.buttons["Campaign"].tap()
        
        // Create new campaign
        app.buttons["Create Campaign"].tap()
        
        // Fill form
        let nameTextField = app.textFields["Campaign Name"]
        nameTextField.tap()
        nameTextField.typeText("Test Campaign")
        
        // Submit
        app.buttons["Create"].tap()
        
        // Verify
        XCTAssertTrue(app.staticTexts["Test Campaign"].exists)
    }
}
```

### **Test Coverage Requirements**
- **Unit Tests**: 80% minimum coverage
- **UI Tests**: Critical user flows
- **Integration Tests**: Firebase and StoreKit integration

## 🔄 Pull Request Process

### **Before Submitting**
1. **Test thoroughly**: Run all tests
2. **Check code style**: Follow Swift style guide
3. **Update documentation**: Add/update relevant docs
4. **Squash commits**: Clean commit history

### **Pull Request Template**
```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
- [ ] Unit tests pass
- [ ] UI tests pass
- [ ] Manual testing completed

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Documentation updated
- [ ] No breaking changes (or documented)

## Screenshots (if applicable)
Add screenshots for UI changes
```

### **Review Process**
1. **Automated Checks**: CI/CD pipeline
2. **Code Review**: At least one approval required
3. **Testing**: Verify functionality
4. **Documentation**: Update relevant docs

## 👀 Code Review Guidelines

### **What to Look For**
- **Code Quality**: Clean, readable code
- **Architecture**: Follows MVVM pattern
- **Security**: User isolation implemented
- **Performance**: Efficient algorithms
- **Testing**: Adequate test coverage

### **Review Comments**
```swift
// ✅ Good: Constructive feedback
// Consider extracting this logic into a separate method for better reusability

// ❌ Bad: Vague feedback
// This doesn't look right
```

### **Review Checklist**
- [ ] Code follows style guide
- [ ] Architecture patterns followed
- [ ] Error handling implemented
- [ ] User isolation maintained
- [ ] Tests included
- [ ] Documentation updated

## 🐛 Issue Reporting

### **Bug Report Template**
```markdown
## Bug Description
Clear description of the issue

## Steps to Reproduce
1. Step 1
2. Step 2
3. Step 3

## Expected Behavior
What should happen

## Actual Behavior
What actually happens

## Environment
- iOS Version: 15.0+
- Device: iPhone 15
- App Version: 1.0.0

## Screenshots/Videos
Add visual evidence

## Additional Context
Any other relevant information
```

### **Feature Request Template**
```markdown
## Feature Description
Clear description of the requested feature

## Use Case
Why this feature is needed

## Proposed Solution
How you think it should be implemented

## Alternatives Considered
Other approaches you've considered

## Additional Context
Any other relevant information
```

## 📚 Resources

### **Development Tools**
- **Xcode**: Primary IDE
- **Git**: Version control
- **Firebase Console**: Backend management
- **App Store Connect**: App distribution

### **Documentation**
- [Swift Style Guide](https://swift.org/documentation/api-design-guidelines/)
- [SwiftUI Documentation](https://developer.apple.com/documentation/swiftui/)
- [Firebase Documentation](https://firebase.google.com/docs)
- [StoreKit Documentation](https://developer.apple.com/documentation/storekit/)

### **Community**
- **GitHub Issues**: Bug reports and feature requests
- **Discussions**: General questions and discussions
- **Code Reviews**: Collaborative development

## 🎯 Contribution Areas

### **High Priority**
- **Bug Fixes**: Critical issues affecting users
- **Security**: User isolation and data protection
- **Performance**: App performance optimizations
- **Testing**: Improving test coverage

### **Medium Priority**
- **New Features**: User-requested functionality
- **UI/UX**: Interface improvements
- **Documentation**: Code and user documentation
- **Refactoring**: Code quality improvements

### **Low Priority**
- **Nice-to-have Features**: Enhancement requests
- **Code Style**: Minor style improvements
- **Comments**: Additional code comments

---

**Thank you for contributing to BD Lead Manager! Your contributions help make this app better for everyone.** 