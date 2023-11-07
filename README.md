#include<SFML/Graphics.hpp>
#include<time.h>
#include <iostream>

using namespace sf;

void gameplay(int m, int n);

int main()
{
    srand(time(0));
    gameplay(20, 20);
 



}

void gameplay(int a, int b){
    RenderWindow app(VideoMode(40*a, 40*b), "Start The Minesweeper");
    //RenderWindow lose(VideoMode(400, 450), "You lose!");
    int w = 32;
    int grid[100][100];
    int sgrid[100][100];

    Texture t;
    t.loadFromFile("images/tiles.jpg");
    Sprite s(t);

    for (int i = 1; i <= a; i++)
        for (int j = 1; j <= b; j++) {
            sgrid[i][j] = 10;

            if (rand() % 5 == 0) grid[i][j] = 9;
            else grid[i][j] = 0;
        }

    for (int i = 1; i <= a; i++)
        for (int j = 1; j <= b; j++) {
            int n = 0;
            if (grid[i][j] == 9) continue;
            if (grid[i + 1][j] == 9) n++;
            if (grid[i][j + 1] == 9) n++;
            if (grid[i - 1][j] == 9) n++;
            if (grid[i][j - 1] == 9) n++;

            if (grid[i + 1][j + 1] == 9) n++;
            if (grid[i - 1][j - 1] == 9) n++;
            if (grid[i - 1][j + 1] == 9) n++;
            if (grid[i + 1][j - 1] == 9) n++;

            grid[i][j] = n;
        }

    while (app.isOpen())
    {
        Vector2i pos = Mouse::getPosition(app);
        int x = pos.x / w;
        int y = pos.y / w;

        Event e;
        while (app.pollEvent(e))
        {
            if (e.type == Event::Closed)
                app.close();

            if (e.type == Event::MouseButtonPressed)
                if (e.key.code == Mouse::Left) {
                    sgrid[x][y] = grid[x][y];
                    if (grid[x][y] == 0) {
                        sgrid[x + 1][y] = grid[x + 1][y];
                        sgrid[x][y + 1] = grid[x][y + 1];
                        sgrid[x - 1][y] = grid[x - 1][y];
                        sgrid[x][y - 1] = grid[x][y - 1];

                        sgrid[x + 1][y + 1] = grid[x + 1][y + 1];
                        sgrid[x - 1][y - 1] = grid[x - 1][y - 1];
                        sgrid[x - 1][y + 1] = grid[x - 1][y + 1];
                        sgrid[x + 1][y - 1] = grid[x + 1][y - 1];
                    }
                }
                else if (e.key.code == Mouse::Right) sgrid[x][y] = 11;

        }

        for (int x = 1; x <= a; x++)
            for (int y = 1; y <= b; y++) {
                if (sgrid[x][y] == 0) {
                    sgrid[x + 1][y] = grid[x + 1][y];
                    sgrid[x][y + 1] = grid[x][y + 1];
                    sgrid[x - 1][y] = grid[x - 1][y];
                    sgrid[x][y - 1] = grid[x][y - 1];

                    sgrid[x + 1][y + 1] = grid[x + 1][y + 1];
                    sgrid[x - 1][y - 1] = grid[x - 1][y - 1];
                    sgrid[x - 1][y + 1] = grid[x - 1][y + 1];
                    sgrid[x + 1][y - 1] = grid[x + 1][y - 1];
                }
            }
        app.clear(Color::White);
        for (int i = 1; i <= a; i++)
            for (int j = 1; j <= b; j++) {

                if (sgrid[x][y] == 9) sgrid[i][j] = grid[i][j];

                s.setTextureRect(IntRect(sgrid[i][j] * w, 0, w, w));
                s.setPosition(i * w, j * w);
                app.draw(s);
            }

        app.display();
    }
}
