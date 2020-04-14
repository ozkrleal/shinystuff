library(shiny)

ui <- fluidPage(
  uiOutput('app')
)

server <- function(input, output) {
  USER <- reactiveValues('Logged' = F)
  credentials <- list('test' = '123', 'ceu' = 'ceudata')
  
  output$app <- renderUI({
    if(USER$Logged == F){
      wellPanel(
        textInput('username', 'Username'),
        textInput('pass', 'Password'),
        actionButton('login', 'Log In')
      )
    } else {
      h1('you are logged in')
    }
    

  })
  
  observeEvent(input$login,{
    if(credentials[[input$username]] == input$pass){
      USER$Logged <- T
    }
    
    
  })
}

#pass = "123"
#user = "test"
shinyApp(ui, server)


#library(safer)
