library(shiny)

source("shiny_function.R")

ui <- pageWithSidebar(
  headerPanel("BMI Calculator (Body Mass Index)"),
  sidebarPanel(
    sliderInput(inputId = 'ht', label = 'Height(cm)', min = 70, max = 200, value = 50, step = 1),
    sliderInput(inputId = 'wt', label = 'Weight(kg)', min = 40, max = 200, value = 50, step = 1),
    
    actionButton(inputId = 'go', label = 'Calculate')
  ),
  mainPanel(
    h3('Results'),
    h4('Your height is:'),
    verbatimTextOutput("oht"),
    h4('Your weight is:'),
    verbatimTextOutput("owt"),
    h4('Your BMI is:'),
    verbatimTextOutput("prediction"),
    h4('Your Index Number is:'),
    textOutput("calculation"),
    h6(em('Reactive output displayed as a result of server calculations.'))
  )
)

BMI = function(height,weight){
  return(round(weight/(height/100)^2))
}

server <- (
  function(input, output) {
    
   # observeEvent(input$wt, {
  #    print(paste('the weigh is: ', input$wt))
  #  })
    
    observeEvent(input$go, {
      print('actionbtn is pushed')
      
      weight <- isolate(input$wt)
      height <- isolate(input$ht)
      bmi_number <- round((input$wt / (input$ht / 100)^2), 2)
      
      #output$prediction <- 
    })
    
  #  my_bmi <- reactive({
  #    print(input$wt)
  #    return(round(input$wt/(input$ht/100)^2))
  #  })
    
    output$oht <- renderPrint({input$ht})
    output$owt <- renderPrint({input$wt})
    output$prediction <- renderPrint({BMI(input$ht, input$wt)})
    #output$calculation <- renderText({get_bmi_by_index_number(BMI(input$ht, input$wt))})
    #output$calculation <- renderText({get_bmi_by_index_number(my_bmi())})
    
    
  }
)



shinyApp(ui, server)
