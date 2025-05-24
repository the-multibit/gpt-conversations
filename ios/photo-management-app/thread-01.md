
# SwiftUI TabView with Custom Icons & Bounce Effect

---

**👤 You:**  
let's create 5 tabs view.. iOS SwiftUI:

- Dashboard  
- Calendar  
- Clients  
- Jobs  
- More

---

**🤖 ChatGPT:**  
Here's a simple TabView structure in SwiftUI with 5 tabs: Dashboard, Calendar, Clients, Jobs, and More.

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        TabView {
            DashboardView()
                .tabItem {
                    Label("Dashboard", systemImage: "house")
                }

            CalendarView()
                .tabItem {
                    Label("Calendar", systemImage: "calendar")
                }

            ClientsView()
                .tabItem {
                    Label("Clients", systemImage: "person.2.fill")
                }

            JobsView()
                .tabItem {
                    Label("Jobs", systemImage: "briefcase.fill")
                }

            MoreView()
                .tabItem {
                    Label("More", systemImage: "ellipsis.circle")
                }
        }
    }
}
```

### Basic Placeholder Views

```swift
struct DashboardView: View {
    var body: some View {
        NavigationView {
            Text("Dashboard")
                .navigationTitle("Dashboard")
        }
    }
}
```

(Same pattern for CalendarView, ClientsView, JobsView, MoreView...)

Let me know if you'd like:

- ✅ Custom icons or SF Symbols  
- ✅ Navigation inside any tab  
- ✅ Sidebar layout for iPad  
- ✅ Figma-style wireframe

---

**👤 You:**  
I want to use custom icons.. and on tap icon should have bounce effect

---

**🤖 ChatGPT:**  
To use **custom icons** and have a **bounce effect on tap** in a `TabView`, you need to:

1. Use `Image("yourIcon")` instead of SF Symbols  
2. Track the selected tab  
3. Animate the tapped icon with a bounce

---

## ✅ Full SwiftUI Example with Custom Icons & Bounce

### 📁 Add icons to `Assets.xcassets`  
(e.g. dashboard.png, calendar.png, etc.)

### 💻 Main Tab Setup

```swift
enum Tab: Int {
    case dashboard, calendar, clients, jobs, more
}

struct ContentView: View {
    @State private var selectedTab: Tab = .dashboard
    @State private var bounceTab: Tab?

    var body: some View {
        ZStack(alignment: .bottom) {
            Group {
                switch selectedTab {
                case .dashboard: DashboardView()
                case .calendar: CalendarView()
                case .clients: ClientsView()
                case .jobs: JobsView()
                case .more: MoreView()
                }
            }
            CustomTabBar(selectedTab: $selectedTab, bounceTab: $bounceTab)
        }
        .edgesIgnoringSafeArea(.bottom)
    }
}
```

---

### 🧩 Custom TabBar View

```swift
struct CustomTabBar: View {
    @Binding var selectedTab: Tab
    @Binding var bounceTab: Tab?

    let icons = [
        ("dashboard", Tab.dashboard),
        ("calendar", Tab.calendar),
        ("clients", Tab.clients),
        ("jobs", Tab.jobs),
        ("more", Tab.more)
    ]

    var body: some View {
        HStack {
            ForEach(icons, id: \.1) { (iconName, tab) in
                Spacer()
                VStack {
                    Image(iconName)
                        .resizable()
                        .scaledToFit()
                        .frame(width: 24, height: 24)
                        .scaleEffect(bounceTab == tab ? 1.2 : 1.0)
                        .animation(.spring(response: 0.3, dampingFraction: 0.4), value: bounceTab)
                    Text(tabLabel(tab))
                        .font(.caption)
                        .foregroundColor(selectedTab == tab ? .blue : .gray)
                }
                .padding(.top, 10)
                .onTapGesture {
                    selectedTab = tab
                    bounceTab = tab
                    DispatchQueue.main.asyncAfter(deadline: .now() + 0.2) {
                        bounceTab = nil
                    }
                }
                Spacer()
            }
        }
        .padding(.vertical, 10)
        .background(Color(UIColor.systemBackground).shadow(radius: 5))
    }

    func tabLabel(_ tab: Tab) -> String {
        switch tab {
        case .dashboard: return "Dashboard"
        case .calendar: return "Calendar"
        case .clients: return "Clients"
        case .jobs: return "Jobs"
        case .more: return "More"
        }
    }
}
```

---

### ✅ Final Notes

- Ensure your image names match those in Assets.
- Customize `scaleEffect` and animation spring as needed.
- Add badges, gradients, or tab icon overlays if required.

Let me know if you want:

- 🔷 Real-time tab preview  
- 🔷 Sidebar layout  
- 🔷 Wireframe view for UI approval
