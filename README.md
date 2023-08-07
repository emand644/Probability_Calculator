import copy
import random

class Hat:
    def __init__(self, **balls):
        self.contents = []
        for ball, count in balls.items():
            self.contents.extend([ball] * count)

    def draw(self, num_balls):
        drawn_balls = []
        if num_balls >= len(self.contents):
            drawn_balls = self.contents[:]
            self.contents.clear()
        else:
            for _ in range(num_balls):
                ball = random.choice(self.contents)
                self.contents.remove(ball)
                drawn_balls.append(ball)
        return drawn_balls

def experiment(hat, expected_balls, num_balls_drawn, num_experiments):
    success_count = 0

    for _ in range(num_experiments):
        copy_hat = copy.deepcopy(hat)
        drawn_balls = copy_hat.draw(num_balls_drawn)
        drawn_balls_dict = {}
        for ball in drawn_balls:
            drawn_balls_dict[ball] = drawn_balls_dict.get(ball, 0) + 1
        
        success = True
        for ball, count in expected_balls.items():
            if drawn_balls_dict.get(ball, 0) < count:
                success = False
                break
        
        if success:
            success_count += 1

    return success_count / num_experiments


# Test example
hat = Hat(black=6, red=4, green=3)
probability = experiment(hat=hat,
                         expected_balls={"red": 2, "green": 1},
                         num_balls_drawn=5,
                         num_experiments=2000)
print(probability)
