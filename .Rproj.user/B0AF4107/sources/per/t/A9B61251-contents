library(devtools)

source('shiny_function.R')
#source_url('bit.ly/ceushiny')


library("shiny")
library("jsonlite")
library("data.table")
library("httr")
library("rtsdata")
library("DT")
library("TTR")
library("plotly")
library("shinyjs")
library("ggplot2")
library("devtools")

# df <- get_data_by_ticker_and_date(ticker = 'TSLA', start_date = Sys.Date() - 365, end_date = Sys.Date())
# 
# get_plot_of_data(df)

ui <- fluidPage(
  uiOutput('ui_tickers'),
  textOutput('selected_ticker_text'),
  dateRangeInput(inputId = 'date', label = 'Select Date', start = Sys.Date() - 365, end = Sys.Date() ),
  #tableOutput('ticker_data')
  DT::dataTableOutput('ticker_data'),
  div(plotlyOutput('data_plotly'), width = '60%', height = '800px', align = 'center'),
  plotOutput("data_ggplot")
)

server <- function(input, output) {
  df <- get_sp500()
  
  output$ui_tickers <- renderUI({
    selectInput(inputId = 'ticker', label = 'Tickers', choices = setNames(df$name, df$description)
                , multiple = FALSE)
  })
  
  output$selected_ticker_text <- renderText(input$ticker)
  
  
   reactive_df <- reactive({
     t <- get_data_by_ticker_and_date(ticker = input$ticker, start_date = input$date[1], end_date = input$date[2])
     #t$date <- as.characters(t$date)
     return(t)
   })
   
   output$ticker_data <- DT::renderDataTable({
     my_render_df(reactive_df())
   })
   
   output$data_plotly <- plotly::renderPlotly(
     {
       get_plot_of_data(reactive_df())
     }
     )
   
   output$data_ggplot <- renderPlot({
     ggplot(reactive_df(), aes(date, close)) + geom_line() + theme_bw()
   })
   # these are the two functions that we need
   #plotly::renderPlotly()
   #plotly::plotlyOutput()
}


# sidebar-layout ----------------------------------------------------------

# ui <- fluidPage(
#   sidebarLayout(
#     sidebarPanel(
#       uiOutput('ui_tickers'),
#       textOutput('selected_ticker_text'),
#       dateRangeInput(inputId = 'date', label = 'Select Date', start = Sys.Date() - 365, end = Sys.Date() )
#     ),
#     mainPanel(
#       tabsetPanel(
#         tabPanel('data', DT::dataTableOutput('ticker_data')),
#         tabPanel('plot', plotlyOutput('data_plotly', width = '60%', height = '800px')),
#         tabPanel('ggplot',plotOutput("data_ggplot"))
#       )
#     )
#   )
# )



# another layout ----------------------------------------------------------


ui <- fluidPage(
  
  div(uiOutput('ui_tickers'), 
        dateRangeInput(inputId = 'date', label = 'Select Date', start = Sys.Date() - 365, end = Sys.Date() )
      , align= "center"),

  tabsetPanel(
    tabPanel('data', DT::dataTableOutput('ticker_data')),
    tabPanel('plot',  div(plotlyOutput('data_plotly', width = '60%', height = '800px')), align = "center"),
    tabPanel('ggplot',plotOutput("data_ggplot"))
  )
  
  
)



# shinythemes ui ----------------------------------------------------------


library(shinythemes)


ui <- navbarPage("Stock Screener", theme = shinythemes::shinytheme('superhero'),
                 tabPanel('data', div(uiOutput('ui_tickers'), 
                                        dateRangeInput(inputId = 'date', label = 'Select Date', start = Sys.Date() - 365, end = Sys.Date() )
                                        , align= "center")),
                 tabPanel('plot', div(plotlyOutput('data_plotly', width = '60%', height = '800px')), align = "center"),
  
  tabsetPanel(
    tabPanel('data', DT::dataTableOutput('ticker_data')),
    tabPanel('ggplot',plotOutput("data_ggplot"))
  )
  
  
)


# another layout swit hshinydashboard -------------------------------------

library(shinydashboard)

ui <- dashboardPage(
  dashboardHeader(title = 'Stocks'),
  dashboardSidebar(
    sidebarMenu(
      menuItem('Control', tabName = 'control', icon = icon('database'),
               uiOutput('ui_tickers'),
               dateRangeInput(inputId = 'date', label = 'Select Date', start = Sys.Date() - 365, end = Sys.Date())
               ), # press f1 to check library
      menuItem('Plot', tabName = 'plot')
    )
      

  ),
  dashboardBody(
    tabItems(
      tabItem(tabName = 'control',
              h1('select date and ticker')
              ),
      tabItem(tabName = 'plot',
              DT::dataTableOutput('ticker_data'),
              plotlyOutput('data_plotly', width = '60%', height = '800px')),
              infoBoxOutput('performance_infobox')
      
    )

  )
)


server <- function(input, output) {
  df <- get_sp500()
  
  output$ui_tickers <- renderUI({
    selectInput(inputId = 'ticker', label = 'Tickers', choices = setNames(df$name, df$description)
                , multiple = FALSE)
  })
  
  output$selected_ticker_text <- renderText(input$ticker)
  
  
  reactive_df <- reactive({
    t <- get_data_by_ticker_and_date(ticker = input$ticker, start_date = input$date[1], end_date = input$date[2])
    #t$date <- as.characters(t$date)
    return(t)
  })
  
  output$ticker_data <- DT::renderDataTable({
    my_render_df(reactive_df())
  })
  
  output$data_plotly <- plotly::renderPlotly(
    {
      get_plot_of_data(reactive_df())
    }
  )
  
  output$data_ggplot <- renderPlot({
    ggplot(reactive_df(), aes(date, close)) + geom_line() + theme_bw()
  })
  # these are the two functions that we need
  #plotly::renderPlotly()
  #plotly::plotlyOutput()
  output$performance_infobox <- renderInfoBox({
    infoBox('Positive performance', paste0(round((sum(df$change>0) / nrow(df) * 100), 0), '%'))
  })
  
}


#renderDataTable()
#dataTableOutput()

shinyApp(ui, server)
