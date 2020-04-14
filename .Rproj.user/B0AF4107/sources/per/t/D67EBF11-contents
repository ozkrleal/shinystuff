library(shiny)

ui <- fluidPage(
  h1('hellow ceu'),
  h2('smaller text'),
  tags$br(),
  tags$hr(),
  tags$code('print()'),
  div(
    img(src = "http://www.lapk.org/images/Buttons/RStudio.png"), 
      height = "140", width = "180", align = 'center'
    ),
  div(h3('change text'), style = "color:blue"),
  
  sliderInput(inputId = 'age', label = 'Age', min = 0, max = 100, value = 50, step = 1),
  #sliderInput(inputId = 'age2', label = 'Age', min = 0, max = 100, value = c(50, 55), step = 1)
  textInput('name', label = 'first text', value = '', placeholder = 'write here'),
  
  textOutput('age_name_text')
)

server <- function(input, output) {
  output$age_name_text <- renderText({
    return(paste0('Name: ', input$name, " Age: ", input$age))
  })
}

shinyApp(ui, server)
