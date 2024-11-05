# Водимобиль

## Описание проекта

Проект представляет собой мобильное приложения для долгосрочной аренды авто в г.Омске. Основные функциональности включают в себя: просмотр всего автопарка "Водимобиль", поиск авто по дате, бронирование авто, логин/регистрация, профиль пользователя. Пользователи могут фильтровать авто по категории, а также по определенному диапазону дат. На экране бронирования пользователь может выбрать дату, время и место приема/отдачи авто, далее заявка отправляется в CRM, где далее обрабатывается менеджером. Приложение интегрировано с CRM «WS автопрокат», которую использует «Водимобиль».


## Назначение проекта

Этот проект был разработан с целью автоматизировать процесс аренды автомобилей в Омске путём создания Android и IOS приложений.  Он решает ряд проблем и удовлетворяет потребности пользователей, связанные с привычным пользованием услугами через цифровые технологии. Платформа позволяет быстро находить и бронировать свободные авто для долгосрочной аренды. Кроме того, детальные страницы с информацией по каждому авто, предоставляют пользователю всё необходимое для создания заявки на бронь.


## Галерея
<img src="https://github.com/ivn-srg/prtf-vodimobile/blob/main/IMG_3759.png" alt="main screen" width="250"><img src="https://github.com/ivn-srg/prtf-vodimobile/blob/main/IMG_3760.png" alt="orders screen" width="250"><img src="https://github.com/ivn-srg/prtf-vodimobile/blob/main/IMG_3761.png" alt="auto list screen" width="250"><img src="https://github.com/ivn-srg/prtf-vodimobile/blob/main/IMG_3762.png" alt="make reservation screen" width="250"><img src="https://github.com/ivn-srg/prtf-vodimobile/blob/main/IMG_3763.png" alt="profile screen" width="250">

## Моя роль в проекте

В рамках данного проекта я единолично отвечал за проектирование и разработку мобильного приложения с iOS стороны. Мои обязанности включали в себя:
<ol>
  <li>проектирование проекта и его кодовой базы,</li>
  <li>реализацию пользовательского интерфейса,</li>
  <li>исправление дефектов,</li>
  <li>обеспечение взаимодействия с общим модулем KMM, отвечающего за серверное взаимодействие.</li>
</ol>
Я тесно сотрудничал с дизайнером и андроид разработчиками, чтобы гарантировать оптимальную реализацию для iOS приложения, оптимизировал UI для обеспечения высокой производительности и отзывчивости интерфейса. Мой вклад в проект также включал обеспечение корректной работы всех функций приложения, отображение всех элементов UI, настройка "моста" для корректной работы с типами данных Kotlin.


## Технологии, используемые в проекте

![Kotlin Multiplatform (KMM)](https://img.shields.io/badge/KMM-0095D5?style=for-the-badge&logo=kotlin&logoColor=white)
![Swift](https://img.shields.io/badge/Swift-FA7343?style=for-the-badge&logo=swift&logoColor=white)
![SwiftUI](https://img.shields.io/badge/SwiftUI-0078D6?style=for-the-badge&logo=swift&logoColor=white)
![Combine](https://img.shields.io/badge/Combine-51A9F5?style=for-the-badge&logo=combine&logoColor=white)
![Jetpack Compose](https://img.shields.io/badge/Compose-3DDC84?style=for-the-badge&logo=jetpackcompose&logoColor=white)
![Ktor](https://img.shields.io/badge/Ktor-0095D5?style=for-the-badge&logo=ktor&logoColor=white)
![Dependency Injection (DI)](https://img.shields.io/badge/DI-7B42BC?style=for-the-badge&logo=dependencyinjection&logoColor=white)
![SwiftLint](https://img.shields.io/badge/SwiftLint-000000?style=for-the-badge&logo=swift&logoColor=white)

## Архитектура проекта

Применена архитектурные паттерны Clean Architecture, MVVM, ориентированные на разделение проекта по функциональным блокам для улучшения масштабируемости и удобства поддержки и тестирования.

<img src="https://github.com/ivn-srg/prtf-vodimobile/blob/main/IMG_3764.png" alt="xcode code structure screen" width="250">


## Принципы и инструменты разработки
- Код-стиль и форматирование: SwiftLint
- Система контроля версий: Github

## Код: Предпросмотр

В данном разделе представлены отрывки кода, демонстрирующие ключевые моменты и методы, использованные в проекте. Эти фрагменты кода отражают стиль программирования, архитектурные решения и технологические практики, применённые в ходе работы.

<details>
  <summary>Главный экран с разделением на вкладки</summary>

  ```swift
  import SwiftUI

  struct MainTabbarView: View {
      @State private var selectedTab: TabType = .main
      @State var showDatePicker: Bool = false
      @ObservedObject var appState = AppState.shared
  
      var body: some View {
          GeometryReader { geometry in
              let tabWidthSize = geometry.size.width / 3
  
              ZStack(alignment: Alignment.bottom) {
                  TabView(selection: $selectedTab) {
                      MainView(
                          selectedTab: $selectedTab,
                          showDatePicker: $showDatePicker)
                          .tag(TabType.main)
                      MyOrdersView(
                          selectedMainTab: $selectedTab,
                          showDatePicker: $showDatePicker
                      )
                      .tag(TabType.myOrders)
                      ProfileView().tag(TabType.profile)
                  }
  
                  HStack(spacing: 0) {
                      TabBarItem(
                          icon: Image(R.image.home),
                          title: R.string.localizable.homeScreenTitle,
                          isSelected: selectedTab == .main,
                          itemWidth: tabWidthSize
                      ) {
                          handleTabSelection(.main)
                      }
  
                      TabBarItem(
                          icon: Image(R.image.car),
                          title: R.string.localizable.myOrdersScreenTitle,
                          isSelected: selectedTab == .myOrders,
                          itemWidth: tabWidthSize
                      ) {
                          handleTabSelection(.myOrders)
                      }
  
                      TabBarItem(
                          icon: Image.personFill,
                          title: R.string.localizable.profileScreenTitle,
                          isSelected: selectedTab == .profile,
                          itemWidth: tabWidthSize
                      ) {
                          handleTabSelection(.profile)
                      }
                  }
                  .frame(maxWidth: .infinity)
                  .background(Color(R.color.container))
              }
              .padding(.vertical, 25)
              .frame(width: geometry.size.width, height: geometry.size.height)
          }
          .ignoresSafeArea()
          .navigationBarBackButtonHidden()
          .fullScreenCover(isPresented: $appState.isInternetErrorVisible) {
              InternetConnectErrorView()
          }
          .onAppear {
              appState.checkConnectivity()
          }
      }
  
      private func handleTabSelection(_ tab: TabType) { selectedTab = tab }
  }
  ```
  
</details>

<details>
  <summary>Компонент список заказов</summary>

  ```swift
  import SwiftUI
  import shared
  
  struct MyOrdersView: View {
      @Binding var selectedMainTab: TabType
      @Binding var showDatePicker: Bool
      @State private var selectedTab: MyOrderTab = .active
      @State var selectedOrder: Order = Order.companion.empty()
      @State var showOrderModal: Bool = false
      @ObservedObject var viewModel = MyOrdersViewModel()
  
      var body: some View {
          NavigationView {
              VStack(spacing: 20) {
                  OrdersTopPickerView(selectedTab: $selectedTab)
  
                  switch selectedTab {
                  case .active:
                      if !viewModel.activeOrderList.isEmpty {
                          OrdersListView(
                              ordersList: $viewModel.activeOrderList,
                              selectedOrder: $selectedOrder,
                              showOrderModal: $showOrderModal
                          ) {
                              await viewModel.getAllOrders()
                          }
                      } else {
                          EmptyOrderListView {
                              await viewModel.getAllOrders()
                          }
                      }
                  case .completed:
                      if !viewModel.completedOrderList.isEmpty {
                          OrdersListView(
                              ordersList: $viewModel.completedOrderList,
                              selectedOrder: $selectedOrder,
                              showOrderModal: $showOrderModal
                          ) {
                              Task {
                                 await viewModel.getAllOrders()
                              }
                          }
                      } else {
                          EmptyOrderListView {
                              Task {
                                 await viewModel.getAllOrders()
                              }
                          }
                      }
                  }
  
                  Spacer()
              }
              .loadingOverlay(isLoading: $viewModel.isLoading)
              .fullScreenCover(isPresented: $showOrderModal, content: {
                  OrderDetailView(
                      order: selectedOrder,
                      showOrderModal: $showOrderModal,
                      selectedTab: $selectedMainTab,
                      showDatePicker: $showDatePicker
                  )
              })
              .padding(.horizontal, horizontalPadding)
              .background(Color(R.color.grayLight))
          }
          .onAppear {
              Task {
                  await viewModel.getAllOrders()
              }
          }
      }
  }
  ```
</details>

<details>
  <summary>Список авто по категориям</summary>
  ```swift
    struct AutoListView: View {
        @Environment(\.calendar) var calendar
        @Binding var selectedAuto: Car
        @Binding var showModalReservation: Bool
        @Binding var showSignSuggestModal: Bool
        @Binding var showDatePicker: Bool
        @State private var selectedTab: Int = 0
        @State private var showModalCard: Bool = false
        @State private var dragOffset: CGSize = .zero
        @ObservedObject private var viewModel: AutoListViewModel
    
        init(
            selectedAuto: Binding<Car>,
            showModalReservation: Binding<Bool>,
            showSignSuggestModal: Binding<Bool>,
            showDatePicker: Binding<Bool>,
            dateRange: Binding<ClosedRange<Date>?>
        ) {
            self._selectedAuto = selectedAuto
            self._showModalReservation = showModalReservation
            self._showSignSuggestModal = showSignSuggestModal
            self._showDatePicker = showDatePicker
            self.viewModel = .init(dateRange: dateRange)
        }
    
        var body: some View {
            VStack {
                if viewModel.dateRange != nil {
                    ButtonLikeDateField(
                        showDatePicker: $showDatePicker,
                        dateRange: viewModel.dateRange
                    )
                    .padding(.horizontal, horizontalPadding)
                }
                TabBarView(index: $selectedTab)
                    .background(
                        RoundedRectangle(cornerRadius: 20)
                            .fill(Color(R.color.background))
                            .ignoresSafeArea(.all)
                    )
    
                TabView(selection: $selectedTab) {
                    switch selectedTab {
                    case 1:
                        ScrollableAutoListView(
                            carList: viewModel.filterCars(by: .economy),
                            selectedAuto: $selectedAuto,
                            showModalCard: $showModalCard,
                            showModalReservation: $showModalReservation,
                            showSignSuggestModal: $showSignSuggestModal,
                            refreshAction: viewModel.fetchCars
                        )
                    case 2:
                        ScrollableAutoListView(
                            carList: viewModel.filterCars(by: .comfort),
                            selectedAuto: $selectedAuto,
                            showModalCard: $showModalCard,
                            showModalReservation: $showModalReservation,
                            showSignSuggestModal: $showSignSuggestModal,
                            refreshAction: viewModel.fetchCars
                        )
                    case 3:
                        ScrollableAutoListView(
                            carList: viewModel.filterCars(by: .premium),
                            selectedAuto: $selectedAuto,
                            showModalCard: $showModalCard,
                            showModalReservation: $showModalReservation,
                            showSignSuggestModal: $showSignSuggestModal,
                            refreshAction: viewModel.fetchCars
                        )
                    case 4:
                        ScrollableAutoListView(
                            carList: viewModel.filterCars(by: .sedans),
                            selectedAuto: $selectedAuto,
                            showModalCard: $showModalCard,
                            showModalReservation: $showModalReservation,
                            showSignSuggestModal: $showSignSuggestModal,
                            refreshAction: viewModel.fetchCars
                        )
                    case 5:
                        ScrollableAutoListView(
                            carList: viewModel.filterCars(by: .jeeps),
                            selectedAuto: $selectedAuto,
                            showModalCard: $showModalCard,
                            showModalReservation: $showModalReservation,
                            showSignSuggestModal: $showSignSuggestModal,
                            refreshAction: viewModel.fetchCars
                        )
                    default:
                        ScrollableAutoListView(
                            carList: $viewModel.listOfAllCar,
                            selectedAuto: $selectedAuto,
                            showModalCard: $showModalCard,
                            showModalReservation: $showModalReservation,
                            showSignSuggestModal: $showSignSuggestModal,
                            refreshAction: viewModel.fetchCars
                        )
                    }
                }
                .ignoresSafeArea(.container)
                .tabViewStyle(PageTabViewStyle(indexDisplayMode: .never))
                .gesture(
                    DragGesture()
                        .onEnded { value in
                            let horizontalAmount = value.translation.width
                            let verticalAmount = value.translation.height
    
                            if abs(horizontalAmount) > abs(verticalAmount) {
                                if horizontalAmount < -50 {
                                    withAnimation {
                                        if selectedTab < AutoListType.allCases.count - 1 {
                                            selectedTab += 1
                                        }
                                    }
                                } else if horizontalAmount > 50 {
                                    withAnimation {
                                        if selectedTab > 0 {
                                            selectedTab -= 1
                                        }
                                    }
                                }
                            }
                        }
                )
                .sheet(isPresented: $showModalCard) {
                    ModalAutoView(
                        carModel: $selectedAuto,
                        showModalView: $showModalCard,
                        showSignSuggestModal: $showSignSuggestModal,
                        showModalReservation: $showModalReservation
                    )
                }
            }
            .onAppear {
                Task {
                    await viewModel.fetchCars()
                }
            }
            .loadingOverlay(isLoading: $viewModel.isLoading)
            .background(Color(R.color.bgContainer))
            .navigationBarBackButtonHidden()
            .toolbar {
                CustomToolbar(title: R.string.localizable.carParkScreenTitle)
            }
        }
    
        func formatDateRange() -> String {
            guard let dateRange = viewModel.dateRange else {
                return R.string.localizable.dateTextFieldPlaceholder()
            }
    
            let formatter = DateFormatter()
            formatter.dateFormat = "dd MMMM yyyy"
    
            let startDate = formatter.string(from: dateRange.lowerBound)
            let endDate = formatter.string(from: dateRange.upperBound)
    
            if startDate == endDate {
                return startDate
            } else if calendar.compare(dateRange.lowerBound, to: dateRange.upperBound, toGranularity: .day) == .orderedAscending {
                return "\(startDate) - \(endDate)"
            } else {
                return "\(endDate) - \(startDate)"
            }
        }
    }
  ```
</details>

<details>
  <summary>Экран бронирования авто</summary>
  ```swift
    struct MakeReservationView: View {
        @Binding var showModal: Bool
        @Binding var selectedTab: TabType
        @Binding var showDatePicker: Bool
        @ObservedObject var viewModel: MakeReservationViewModel
        @State private var navigationPath = NavigationPath()
        @Environment(\.dismiss) private var dismiss
    
        enum Destination: Hashable {
            case successView
            case failureView
        }
    
        init(
            car: Car,
            selectedTab: Binding<TabType>,
            dates: ClosedRange<Date>? = nil,
            showModal: Binding<Bool>? = nil,
            showDatePicker: Binding<Bool>
        ) {
            self.viewModel = .init(car: car, dates: dates)
            self._selectedTab = selectedTab
            self._showModal = showModal ?? Binding.constant(false)
            self._showDatePicker = showDatePicker
        }
    
        var body: some View {
            NavigationView {
                ZStack(alignment: .top) {
                    VStack {
                        HStack {
                            Button(action: {
                                showModal.toggle()
                                dismiss()
                            }, label: {
                                Image.chevronLeft
                                    .foregroundStyle(Color(R.color.text))
                                    .fontWeight(.bold)
                            })
                            Text(R.string.localizable.reservationScreenTitle)
                                .font(.header1)
                                .foregroundStyle(Color(R.color.text))
                                .frame(maxWidth: .infinity)
                        }
                        ScrollView(.vertical, showsIndicators: false) {
                            VStack(alignment: .leading, spacing: 24) {
                                HStack {
                                    viewModel.carPreview
                                        .resizable()
                                        .aspectRatio(contentMode: .fit)
                                        .frame(maxWidth: screenWidth / 2.3)
    
                                    Spacer()
    
                                    VStack(alignment: .leading, spacing: 12) {
                                        VStack(alignment: .leading) {
                                            Text(R.string.localizable.autoNameTitle)
                                                .font(.paragraph5)
                                                .foregroundStyle(Color(R.color.grayText))
                                            Text(viewModel.car.model.resource)
                                                .font(.header5)
                                        }
    
                                        if let dates = viewModel.dates {
                                            VStack(alignment: .leading) {
                                                Text(R.string.localizable.autoDatesTitle)
                                                    .font(.paragraph5)
                                                    .foregroundStyle(Color(R.color.grayText))
                                                Text(dates).font(.header5)
                                            }
                                        }
                                    }
                                    .multilineTextAlignment(.leading)
                                }
                                .padding(.horizontal, horizontalPadding)
                                .padding(.vertical, 24)
                                .background(
                                    RoundedRectangle(cornerRadius: 16)
                                        .fill(Color(R.color.blueBox))
                                )
    
                                if viewModel.dates == nil {
                                    ButtonLikeBorderedTextField(
                                        fieldType: .datePicker,
                                        showDatePicker: $showDatePicker,
                                        inputErrorType: $viewModel.inputErrorType,
                                        dateRange: $viewModel.dateRange
                                    )
                                }
    
                                ButtonLikeBorderedTextField(
                                    fieldType: .startPlacePicker,
                                    inputErrorType: $viewModel.inputErrorType,
                                    selectedPlace: $viewModel.startPlace,
                                    placesDataSource: $viewModel.placesWithCost
                                )
    
                                ButtonLikeBorderedTextField(
                                    fieldType: .startTimePicker,
                                    inputErrorType: $viewModel.inputErrorType,
                                    time: $viewModel.startTime,
                                    showTimePicker: $viewModel.showStartTimePicker
                                )
    
                                ButtonLikeBorderedTextField(
                                    fieldType: .endPlacePicker,
                                    inputErrorType: $viewModel.inputErrorType,
                                    selectedPlace: $viewModel.endPlace,
                                    placesDataSource: $viewModel.placesWithCost
                                )
    
                                ButtonLikeBorderedTextField(
                                    fieldType: .endTimePicker,
                                    inputErrorType: $viewModel.inputErrorType,
                                    time: $viewModel.endTime,
                                    showTimePicker: $viewModel.showEndTimePicker
                                )
    
                                HorizontalServicesScrollView(
                                    servicesList: $viewModel.servicesList,
                                    selectedServicesList: $viewModel.selectedServices
                                )
                                Spacer()
                            }
                        }
    
                        VStack(spacing: 20) {
                            HStack {
                                Text(R.string.localizable.totalPriceTitle)
                                    .font(.header3)
                                Spacer()
                                Text("\(Int(viewModel.bidCost)) \(R.string.localizable.currencyText())")
                                    .font(.header3)
                            }
    
                            NavigationStack(path: $navigationPath) {
                                VStack {
                                    Button(R.string.localizable.leaveReuqestButton(), action: {
                                        Task {
                                            await viewModel.createBidToReserve()
                                        }
                                    })
                                    .buttonStyle(FilledBtnStyle())
                                    .disabled(
                                        viewModel.startPlace == nil &&
                                        viewModel.endPlace == nil &&
                                        viewModel.dateRange == nil
                                    )
                                }
                                .navigationDestination(for: Destination.self) { destination in
                                    switch destination {
                                    case .successView:
                                        SuccessfulReservationView(
                                            showModal: $showModal,
                                            selectedTab: $selectedTab
                                        )
                                    case .failureView:
                                        FailureReservationView(showModal: $showModal) {
                                            _ = await viewModel.createBidToReserve()
                                        }
                                    }
                                }
                            }
                            .frame(maxHeight: 100)
                        }
                        .padding(.horizontal, 10)
                        .padding(.vertical, 20)
                    }
                    .padding(.horizontal, horizontalPadding)
    
                    if viewModel.showStartTimePicker {
                        ModalTimePicker(
                            selectedTime: $viewModel.startTime,
                            showTimePicker: $viewModel.showStartTimePicker
                        )
                    } else if viewModel.showEndTimePicker {
                        ModalTimePicker(
                            selectedTime: $viewModel.endTime,
                            showTimePicker: $viewModel.showEndTimePicker
                        )
                    }
                }
            }
            .loadingOverlay(isLoading: $viewModel.isLoading)
            .datePickerModalOverlay(
                showDatePicker: $showDatePicker,
                dateRange: $viewModel.dateRange
            )
            .navigationBarBackButtonHidden()
            .fullScreenCover(isPresented: $viewModel.showSuccessModal) {
                SuccessfulReservationView(showModal: $showModal, selectedTab: $selectedTab)
            }
            .fullScreenCover(isPresented: $viewModel.showErrorModal) {
                FailureReservationView(showModal: $showModal) {
                    _ = await viewModel.createBidToReserve()
                }
            }
        }
    }
  ```
</details>

## Команда
- Общее количество человек: 8
- Роли в команде:
  - Разработчиков: 4
  - Дизайнеров: 1
  - Project manager: 1
  - QA: 1
  - Marketing: 1

# Основные достижения и результаты

- Успешная интеграция UI и общего сетевого модуля проекта, обеспечивающая бесперебойную работу всего приложения.
- Реализация интуитивно понятного пользовательского интерфейса, который позволяет приятно отображать весь автопарк.
- Оптимизация производительности UI части, гарантирующая понятную и отзывчивую работу приложения.
- Создание детальных страниц авто, предоставляющих всю необходимую информацию в удобном формате.
- Повышение общей эффективности работы менеджеров Водимобиль, за счет сокращения времени личного взаимодействия с клиентами.


## Особые вызовы и преодоленные препятствия

- **Интеграция с общим сетевым модулем**: Столкнулись с трудностями совместимости типов данных Kotlin и Swift, решили через добавление сторонней библиотеки Skie.
- **Производительность интерфейса**: Изначальные проблемы с отзывчивостью решены оптимизацией кода и использованием ленивой загрузки.
- **Слабая безопасность**: Существующее API и взаимодействие с ним было открытым и небезопасным, было решено через внедрение готового коробочного прокси сервера Supabase.


## Ссылки

- **Демо проекта**: Доступ к демо-версии проекта строго ограничен и предоставляется исключительно клиентам компании по запросу.
- **Код проекта**: Код находится под защитой соглашения о неразглашении (НДА), из-за чего, к сожалению, не может быть предоставлен для общего доступа или просмотра.
